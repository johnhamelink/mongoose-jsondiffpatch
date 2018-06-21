mongoose-jsondiffpatch
=============

Mongoose collections history using jsondiffpatch

[![npm](https://img.shields.io/npm/v/mongoose-jsondiffpatch.svg)](https://www.npmjs.com/package/mongoose-jsondiffpatch)
[![GitHub license](https://img.shields.io/github/license/t4nz/mongoose-jsondiffpatch.svg)](https://github.com/t4nz/mongoose-jsondiffpatch/blob/master/LICENSE)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/5f0f54069d254079bdf9e5c71eb7debc)](https://www.codacy.com/app/t4nz/mongoose-jsondiffpatch?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=t4nz/mongoose-jsondiffpatch&amp;utm_campaign=Badge_Grade)
[![npm](https://img.shields.io/npm/dm/mongoose-jsondiffpatch.svg)](https://www.npmjs.com/package/mongoose-jsondiffpatch)
[![TypeScript](https://badges.frapsoft.com/typescript/version/typescript-next.svg?v=101)](https://github.com/ellerbrock/typescript-badges/)

## Installation
---------------
``` sh
yarn add mongoose-jsondiffpatch
npm i mongoose-jsondiffpatch
```

## Operation
---------------
Each update will create a history record with [jsonDiff](https://github.com/benjamine/jsondiffpatch) of the change. This helps in tracking all the changes happened to an object from the beginning.

Following will be the structure of the diff history being saved:


diff Collection schema:

```
_id: ObjectId - mongo id of the diff object
collectionName: string - Name of the collection for which diff is saved
collectionId: ObjectId - Mongo Id of the collection being modified
diff: object - Diff object
user: any - User who modified
reason: string - Why the collection is modified
createdAt: Date
updatedAt: Date
_v: number
```

## Setup
---------------
Use as you would any Mongoose plugin:

```js
import * as mongoose from 'mongoose';
import mongooseJsondiff from 'mongoose-jsondiffpatch';

const schema = new mongoose.Schema({ ... });
schema.plugin(mongooseJsondiff, { mongoose });
const model = model('MyModel', schema);
```

The plugin also has an omit option which accepts either a string or array. This will omit the given
keys from history. Follows dot syntax for deeply nested values.

```js
import * as mongoose from 'mongoose';
import mongooseJsondiff from 'mongoose-jsondiffpatch';

const schema = new mongoose.Schema({
    someField: String,
    ignoredField: String,
    some: {
        deepField: String
    }
});

schema.plugin(diffHistory.plugin, { mongoose, omit: ['ignoredField', 'some.deepField'] });
...
```

## API
* [`getDiffs()`](#getDiffs)
* [`getVersion()`](#getVersion)

### getDiffs()
Returns raw histories created for a document.


```js
async () => {
  try {
    import model from './myModel';
    // with optional query parameters
    const histories = await model.getDiffs(ObjectId, { select: 'diff user' });
    ...
```

### getVersion()
Returns an older version of a document.
```js
async () => {
  try {
    import model from './myModel';
    // with optional query parameters
    const version = await model.getVersion(ObjectId, version, { lean: true });
    ...
```

## Credits
This plugin is inspired by [mimani/mongoose-diff-history](https://github.com/mimani/mongoose-diff-history).

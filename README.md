# Pisco Unit Tests - Test recipe.

Writing a test for piscosour. First require stepTester

```javascript
 const tester = require('piscosour/lib/tests/stepTester');
```

1. [command object](#command)
1. [setLoggerLevel(level) method](#setLoggerLevel)
1. [loadStep(command) method](#loadStep)
1. [runStep(command) method](#runStep)

#### <a name="command"></a>command object.

| Param | Type | Optional | Description |
| --- | --- | --- | --- |
| name | String | No | The name of the step  |
| context | Array | Yes | Array of the contexts runned |
| baseDir | String | Yes | Folder where the step is going to be runned. Default is the root of the recipe. |
| params | Object | Yes | All params passed for the execution |

Is the configuration parameter that piscosour use to run the test.

#### <a name="setLoggerLevel"></a>setLoggerLevel(level) method

`logger.setLoggerLevel(level)` set logger level for piscosour. [See logger for more information](./14-logger.md)

| Param | Type | Optional | Description |
| --- | --- | --- | --- |
| level | Number | No | 0...5 log levels  |

#### <a name="loadStep"></a>loadStep(command) method

`logger.loadStep(command)` Load local step with all plugins and all piscosour configuration. **Returns** step object with all piscosour features loaded.

| Param | Type | Optional | Description |
| --- | --- | --- | --- |
| command | Object | No | See [command object](#command) |


#### <a name="runStep"></a>runStep(command) method

`logger.runStep(command)` Run one local step from the recipe. **Returns** a promise with the execution of the step.

| Param | Type | Optional | Description |
| --- | --- | --- | --- |
| command | Object | No | See [command object](#command) |

This is a example of a entire test file.

```javascript
'use strict';

const path = require('path');
const tester = require('piscosour/lib/tests/stepTester');
const expect = require('chai').expect;
const assert = require('assert');

/* global define, it, describe, before, beforeEach, afterEach, after */

// configure

tester.setLoggerLevel(0);

// constants

const stepName = 'askHello';
const contexts = ['world'];
const message = 'Unit Framework hola!';

describe('Unit testing framework for askHello step', () => {
  it('Should return the step to test', (done) => {
    const step = tester.loadStep({
      name: stepName,
      context: contexts
    });
    expect(step).not.equal(null);
    expect(step.name).to.equal(stepName);
    done();
  });
  it('Should run the step to test', (done) => {
    tester.setLoggerLevel(0);
    tester.runStep({
        name: stepName,
        context: contexts,
        baseDir: path.join(__dirname, 'world'),
        params: {
          paramInquire: message
        }
      })
      .then(() => {
        done();
      })
      .catch((err) => {
        done(err);
      })
  });
  it('Should run the step with plugins to test', (done) => {
    tester.setLoggerLevel(0);
    tester.runStep({
        name: 'emittingHello',
        context: contexts,
        baseDir: path.join(__dirname, 'world'),
        params: {
          paramInquire: message
        }
      })
      .then(() => {
        done();
      })
      .catch((err) => {
        done(err);
      })
  });
});

```

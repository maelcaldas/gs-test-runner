[TDD]: http://en.wikipedia.org/wiki/Test-driven_development
[GSUnit]: https://sites.google.com/site/scriptsexamples/custom-methods/gsunit
[CI]: http://en.wikipedia.org/wiki/Continuous_integration
[Google Apps Script]: https://developers.google.com/apps-script/
[Development Mode]: https://developers.google.com/apps-script/guide_libraries#testAndDebug


# GSTestRunner  [![GSTests status](https://gs-tests-status.appspot.com/badge.svg?suite=GSTestRunner&namespace=bkper)](https://script.google.com/macros/s/AKfycbyWJJFIwoqnNudRMGse18qVNWw5aa7g03-iLmL_rjqO8mg-MjI/exec?suite=GSTestRunner&namespace=bkper)

[Google Apps Script] library to RUN tests, PUBLISH results and send email NOTIFICATIONS of failures.

GSTestRunner helps with [TDD] and also set a lightweight [CI] for your GS code.

This library does NOT provide assertions. For that, we suggest [GSUnit].

#Get Started

Add the library through project key: **Mv3gVv8-YsY7WWlQhoAGWgpNuZToV6OsP**

Create a script with test methods starting with "test":

````javascript

function runSuite() {
  GSTestRunner.runSuite(this, "Get Started");
}

function testA_() {
  Logger.log("Good code!");
}

function testB_() {
  Logger.log("Bad code!");
  nonExistingFunction();
}


````

Then, run the suite passing `this` as the suite object, and set a trigger to periodically run the test suite, depending on your needs - every hour would be good ;)

The suite result based on code above can be found here: [![GSTests status](https://gs-tests-status.appspot.com/badge.svg?suite=Get Started&namespace=bkper)](https://script.google.com/macros/s/AKfycbyWJJFIwoqnNudRMGse18qVNWw5aa7g03-iLmL_rjqO8mg-MjI/exec?suite=Get Started&namespace=bkper)

If you are testing another library, don't forget to let the [Development Mode] on, so the test will run always on latest code version.

**PS**: The suiteName must be unique. If someone already taken the name, you will get an error like this:

 `Suite name "Get Started" already taken! Change the name and/or namespace, or ask one of those guys to include you as recipient: [xxx@gmail.com]`

Just follow the instructions to find an available suite name, or to get your email added to an already taken one.


#Befores and Afters

Sometimes you need to setup and cleanup fixtures before and after tests. If implemented, GSTestRunner run the following functions, in respective order:

Function      | Order
------------- | -------------
beforeSuite_  | Before all tests
beforeTest_   | Before each test
afterTest_    | After each test
afterSuite_   | After all tests

Example:

````javascript

//Befores and Afters
function beforeSuite_() {
  Logger.log("Run before all tests");
}
function beforeTest_() {
  Logger.log("Run before each test");
}

function afterTest_() {
  Logger.log("Run after each test");
}
function afterSuite_() {
  Logger.log("Run after all tests");
}

````


#Options

When calling `GSTestRunner.runSuite` You can pass some options as the third parameter, that can change the behavior:


  option    |  Type   | What it does
----------- | ------- | ------------
namespace   | string  | Define a namespace to avoid suite naming colisions
notify      | boolean | Send email notifications to recipient when test fails
recipient   | string  | Comma separated emails to receive failure notifications
errorOnFail | boolean | Throw error when suite fails. Good for development.
testCodeUrl | string  | The URL of test script code, to be linked on results

Example:

````javascript

function runSuite(event) {

  //Trick to know if is triggered by time trigger
  var isRunningByTimeTrigger = event != null;

  var options = {
    namespace: "bkper",
    notify: isRunningByTimeTrigger,
    recipient: "bkper-dev@googlegroups.com",
    errorOnFail: !isRunningByTimeTrigger,
    testCodeUrl: "https://script.google.com/a/nimbustecnologia.com.br/d/19IiyKv3t5WlqDLcDWwMO8Y_eBeWaNJyP9kZiPSGECT8GCrbFlBw_28B-/edit",
  }

  GSTestRunner.runSuite(this, "Get Started");
}

````
Take a look at the trick to know if the suite is running by a time trigger, or manually. Its good to avoid unnecessary email notifications when on development.

#Suite Result

You can see the results of a test suite by checking the log after calling `GSTestRunner.runSuite`.

Everytime you run a test suite, the results are stored for later retrieve. You can check the logs to see the results, or retrieve it anytime by calling `GSTestRunner.getSuiteResult`:

````javascript

function logSuiteResult() {
  var suiteResult = getSuiteResult("Get Started", "bkper");
  Logger.log("status: " + suiteResult.status);
  Logger.log("total: " + suiteResult.total);
  Logger.log("failed: " + suiteResult.failed);
  Logger.log("passed: " + suiteResult.passed);

  Logger.log("URL: " + suiteResult.url);

}

````
Results are also published to a simple web view, and can be accessed through the url logged above.

#Running a single test

You can run a single test by calling `GSTestRunner.runTest`:

````javascript

function runASingleTest() {
  GSTestRunner.runTest(this, "testA_");
}

function testA_() {
  Logger.log("Good code!");
}

````
When you run a single test, the results are NOT stored.

#Status Badges




#Complete Example




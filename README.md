# poc-coverage-test #

This is a Proof of Concept to verify that APIKit flows are not ignored under MUnit Code Coverage

## Steps to reproduce ##

1. Clone this repo
2. Run `mvn clean verify`
    * This will pull down the project dependencies and run the MUnit test suite
3. The build should fail because it does not meet 100% code coverage requirement
4. Open the MUnit Coverage report under [target/munit-reports/coverage/summary.html](/target/munit-reports/coverage/summary.html)
    * Observe that `poc-coverage-implementation:/get-greeting-implementation` has 100% Code Coverage
    * Observe that `poc-coverage-test-main`, `poc-coverage-test-console`, and `poc-coverage-test-apiKitGlobalExceptionMapping` are ignored for code coverage. As is expected
    * Observe that `get:/greeting:poc-coverage-test-config` is not ignored for code coverage. However, it should be.

## Expectations ##

* The build should pass (Currently it fails because it does not meet 100% Code Coverage)
* The build should have 100% Code Coverage

# Steps to resolve

It appears that the `ignoreFlow` uses regex to match on the name of the flow. Since `/` is an invalid character on it's own in a regex, you need to escape it.
See [regexr.com](regexr.com/44is1) for example

1. Ensure that your flows are escaped to allow for proper regex matching
    * Change from `get:/greeting:poc-coverage-test-config` to `get:\/greeting:poc-coverage-test-config`
    * Re-run `mvn clean verify`
    * Observe that the build passes due to 100% code coverage
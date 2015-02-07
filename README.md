#JUnit-Karma test suite runner#
This library allows you to run a karma test suite with a java JUnit test runner.

************
A full running demo for using the runner can be found at https://github.com/FlorianGrundig/demo-junit-karma-testrunner
************

###History###
<pre>
1.5-snapshot:
* important: reinstall node modul karma-remote-reporter to v0.2.x
* receiving test results via raw tcp socket communication instead of websockets
</pre>
<pre>
1.4-snapshot:
* removed java 1.7 specific stuff to be compatible with java 1.6
</pre>
<pre>
1.3-snapshot:
* added maven-shade-plugin to the package phase to build an uber-jar which includes all required dependencies
* important: reinstall karma-remote-reporter to get the latest version, which is now compatible to karma 0.12.x
</pre>
<pre>
1.2-snapshot:
* important: reinstall karma-remote-reporter to get the latest version, which fixes a bug when running multiple junit-karma-testrunner simultaneously without getting a "port already in use"-exception
</pre>

<pre>
1.1-SNAPSHOT:
* IMPORTANT: reinstall karma-remote-reporter to get the latest version, which fixes critical reporting errors
* the JUnit testrunner will fail, when karma reports syntax errors
</pre>


###Prerequisites###
To combine the javascript karma test runner with a java JUnit runner you need this library and our
<a href="http://github.com/ImmobilienScout24/karma-remote-reporter" target="_blank">karma-remote-reporter</a>.
The karma-remote-reporter is a karma plugin which you can add easily in your "karma.conf":
<pre><code>
module.exports = function (config) {
    config.set({
      basePath: '../.',
        files: [
            'lib/angular/angular.js',
            'js/*.js',
            'unit-tests/**/*.js'
        ],

        browsers: ['PhantomJS'],
        reporters: ['junit','remote'],
        frameworks: ["jasmine"],
        autoWatch: false,
        singleRun: true,
        junitReporter: {
            outputFile: 'target/test_out/unit.xml',
            suite: 'unit'
        },
        remoteReporter: {
            host: 'localhost',
            port: '9889'
        },
        plugins: [
            'karma-jasmine',
            'karma-phantomjs-launcher',
            'karma-junit-reporter',
            'karma-remote-reporter'
        ]
    });
};
</code></pre>
To add the remote reporter plugin add 'karma-remote-reporter' to the plugin section and add 'remote' to the reporters section.
To configure the host and port where the karma remote reporter should send the test results use the remoteReporter section.

The test results will send by the remote reporter to a server which is in the java world e.g. this JUnit-karma-testrunner.
All you need is a normal java test class:

###JavaScriptTestSuite.java###
<pre><code>
@RunWith(KarmaTestSuiteRunner.class)
@KarmaTestSuiteRunner.KarmaConfigPath("path.to.your.karma.conf") // use several test classes with different karma configurations for e.g. running unit or e2e tests
public class KarmaTestSuiteRunnerBasicTestFixture {

    @BeforeClass
    public static void setupTestSzenario(){
     ...
    }
    @AfterClass
    public static void cleanupTestSzenario(){
     ...
    }
}

</code></pre>
With the KarmaTestSuiteRunner annotations you can configure your the server where the test results will be reported and the karma config.
With @BeforeClass and @AfterClass you can setup and cleanup your test scenario when running e2e tests.

There're several more configuration points using the annotations in KarmaTestSuiteRunner:
de.is24.util.karmatestrunner.junit.KarmaTestSuiteRunner.KarmaProcessName -> defaults to "karma" for linux and mac os, "karma.cmd" for windows os

de.is24.util.karmatestrunner.junit.KarmaTestSuiteRunner.KarmaProcessArgs -> defaults to "start"

de.is24.util.karmatestrunner.junit.KarmaTestSuiteRunner.KarmaStartupScripts -> you can setup the karma environment e.g. path setup or browser bin location variables by yourself via a shell or batch script

de.is24.util.karmatestrunner.junit.KarmaTestSuiteRunner.KarmaRemoteServerPort -> defaults to 9889

You can overwrite all annotations by using the following system properties:

  karma.process.name

  karma.process.args

  karma.startup.scripts

  karma.remoteServerPort


With the system properties you can overwrite the environment specifics to run the tests on a dev machine and a ci server without code changes...





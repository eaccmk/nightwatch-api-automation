nightwatch-api-automation


```bash
mkdir nightwatch-api-automation
```



[Install Nightwatch](https://nightwatchjs.org/guide/quickstarts/create-and-run-a-nightwatch-test.html)

```bash
npm init nightwatch nightwatch-api-automation
```

```bash
npm i @nightwatch/apitesting --save-dev
```

Go to **nightwatch.conf.js** 

change `plugins: [],` to ` plugins: ['@nightwatch/apitesting'],`

In same config file (_nightwatch.conf.js_)

under `test_settings: {}` , after `default: {}` add a new object [ config] like this ( you may copy paste )


```bash
api_testing: {
	  start_session: false,
	    webdriver: {
	      start_process: false,
	  }
	},
```



So 
```
    webdriver: {
        start_process: true,
        server_path: ''
      },
      
    },
    
    chrome: {
      desiredCapabilities: {
```

becomes

```
    webdriver: {
        start_process: true,
        server_path: ''
      },
      
    },

	api_testing: {
	  start_session: false,
	    webdriver: {
	      start_process: false,
	  }
	},
    
    chrome: {
      desiredCapabilities: {
```


now lets create a simple API test file and run it:

creat a file under `test/API/` named `api-test.js` and create a test, or try with this ( copy paste)
ref : https://github.com/nightwatchjs/nightwatch-docs/blob/versions/3.0/guide/writing-tests/api-testing.md#test-api-headers--responses

```javascript
describe('api testing', function () {
  it('get api test', async function({supertest}) {
    await supertest
      .request("https://petstore.swagger.io/v2")
      .get("/pet/findByStatus?status=available")
      .expect(200)
      .expect('Content-Type', /json/)
      .then(function(response){
          expect(response._body.length).to.be.greaterThan(0);
      });
  });
});
```

run it

```bash
npx nightwatch test/API/api-test.js --env api_testing
```

> `api_testing` tells it to run API testing only [ not browser testing as we configured in above steps]

You can also add a short command in `package.json` file like I added for this project :

```bash
npm test
```


now to see API tetsing in action


```bash
$ npx nightwatch test/API/api-test.js --env api_testing


[api testing] Test Suite
────────────────────────────────────────────────

  Running get api test:
──────────────────────────────────────────────────────────────────────────────────────────────────────────
  ✔ Passed [ok]: .get('/pet/findByStatus?status=available').expect(200) ok
  ✔ Passed [ok]: .get('/pet/findByStatus?status=available').expect('Content-Type', /json/) ok
  ✔ Expected 833  to be greaterThan(+0):

  ✨ PASSED. 3 assertions. (619ms)
 Wrote HTML report file to: ./nightwatch-api-automation/tests_output/nightwatch-html-report/index.html

```


nightwatch_report {HTML}



```
"@nightwatch/apitesting" : {
    "log_responses": true
  }
```

So, go back to file
lets say we want to add new config after `test_workers` so 


this 
```
  test_workers: {
    enabled: true
  },

  test_settings: {
    default: {

```
becomes

```
  test_workers: {
    enabled: true
  },

  "@nightwatch/apitesting" : {
    "log_responses": true
  },

  test_settings: {
    default: {
```
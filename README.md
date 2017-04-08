# node-fetch-response-matchers

[![Build Status](https://travis-ci.org/kfiron/node-fetch-response-matchers.svg?branch=master)](https://travis-ci.org/kfiron/node-fetch-response-matchers)

Matchers for node-fetch promise response.
It helps the tests to be more declarative.

## TL;DR

- Verbose way to test node-fetch response:
```javascript
   it('some-test', function(done){
      fetch('http://localhost/')
         .then(res => {
            expect(res.status).to.equal(200);
            return res.text();
         }).then(text => {
            expect(text).to.equal('foo');
            done();
         })
   });
```

- Using this lib in more declarative way with hide the promise complexity:
```javascript
   it('some-test', function(){
     return expect(fetch('http://localhost/')).to.be.successful()
                            .and.to.haveBodyText('foo');

   });
```



## Install (for dev only - used by tests)
```shell
$ npm i node-fetch-response-matchers --save-dev
```

## Usage example
```javascript

const nodeFetchMatchers = require('node-fetch-response-matchers');
const fetch = require('node-fetch');
const chai = require('chai');

chai.use(nodeFetchMatchers);

describe('test suite', function(){
    it('http success test', function(){
        return expect(fetch('http://localhost/')).to.be.successful();
    });
});
```

## API description
You can all use chai "not" and compose by chai "and", for example

```javascript
   it('not', function(){
      return expect(fetch('http://localhost/')).to.not.be.successful();
   });
   it('and', function(){
      return expect(fetch('http://localhost/')).to.be.successful()
                                                .and.haveBodyText('foo');
   });
```


## Status matchers

```javascript
   it('http success test', function(){
      return expect(fetch('http://localhost/')).to.be.successful();
   });
   it('http status assert', function(){
        return expect(fetch('http://localhost/')).to.haveStatus(500);
   });
```

#### Full status matchers list

| API function         | params   | description                      |
| ---------------------|----------| ---------------------------------|
| successful()         | ()       | Assert that the status is 200 OK |
| created()            | ()       | Assert that the status is 201    |
| unauthorized()       | ()       | Assert that the status is 401    |
| rejected()           | ()       | Assert that the status is 403    |
| notFound()           | ()       | Assert that the status is 404    |
| serverError()        | ()       | Assert that the status is 500    |
| serviceUnAvailable() | ()       | Assert that the status is 503    |
| haveStatus()         | (status) | Assert that the status is provided number argument    |



## Body matchers

```javascript
   it('have body object', () => {
     return expect(fetch('http://localhost/').to.be.haveBodyObject({foo: 'bar'});
   });
```
#### Full body matchers list

| API function         | params             | description                                                  |
| ----------------------|-------------------| -------------------------------------------------------------|
| haveBodyObject()      | (obj)             | Assert equal provided object                                 |
| haveBodyText()        | (text)            | Assert equal provided string text                            |
| haveBodyRegexpMatch() | (regexp)          | Assert match body on regular expression                      |
| haveBodyThat()        | (predicate(text)) | Assert match body on provided function predicate on the text |



## Header matchers

```javascript
   it('have header', () => {
     return expect(fetch('http://localhost/').to.be.haveHeader('connection', 'close');
   });
```
#### Full headers matchers list

| API function  | params         | description                                                     |
| --------------|----------------| ----------------------------------------------------------------|
| haveHeader()  | (name, value)  | Assert that response contains header by provided name and value |
| headerExists()| (name)         | Assert that response contains header by provided name           |

## Cookie matchers

```javascript
   it('have cookie', () => {
     return expect(fetch('http://localhost/').to.be.haveCookie('foo', 'bar');
   });
```
#### Full cookie matchers list


| API function       | params         | description                                                     |
| -------------------|----------------| ----------------------------------------------------------------|
| haveCookieByName() | (name)         | Assert that cookie by name is written to the response           |
| haveCookie()       | (name, value)  | Assert that cookie by name and value is written to the response           |


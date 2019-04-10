# react-hooks-axios

[![NPM version][npm-image]][npm-url]
[![npm download][download-image]][download-url]
[![Build Status][travis-image]][travis-url]

Custom React Hooks for Axios.js

## Install

>**Note:** Make sure that you have installed the correct version of `react(>= v16.8.0)` and `react-dom(>= v16.8.0)`.

### npm

```bash
npm install --save @use-hooks/axios
```

### yarn

```bash
yarn add @use-hooks/axios
```

## API

### Params

```js
/**
 * Params
 * @param  {string} url - The request URL
 * @param  {('GET'|'POST'|'PUT'|'DELETE'|'HEAD'|'OPTIONS'|'PATCH')} method - The request method
 * @param  {object} [options={}] - (optional) The config options of Axios.js (https://goo.gl/UPLqaK)
 * @param  {object|string} trigger - (optional) The conditions for AUTO RUN, refer the concepts of [conditions](https://reactjs.org/docs/hooks-reference.html#conditionally-firing-an-effect) of useEffect, but ONLY support string and plain object. If the value is a constant, it'll trigger ONLY once at the begining
 * @param  {function} [forceDispatchEffect=() => true] - (optional) Trigger filter function, only AUTO RUN when get `true`, leave it unset unless you don't want AUTU RUN by all updates of trigger
 * @param  {function} [customHandler=(error, response) => {}] - (optional) Custom handler callback, NOTE: `error` and `response` will be set to `null` before request
 */
```

### Returns

```js
/**
 * Returns
 * @param  {object} response - The response of Axios.js (https://goo.gl/dJ6QcV)
 * @param  {object} error - HTTP error
 * @param  {boolean} loading - The loading status
 * @param  {function} reFetch - MANUAL RUN trigger function for making a request manually
 */
```

## Usage

```js
import React, { useState } from 'react';

import useAxios from '@use-hooks/axios';

export default function App() {
  const [gender, setGender] = useState('');
  const { response, loading, error, reFetch } = useAxios({
    url: `https://randomuser.me/api/${gender === 'unknown' ? 'unknown' : ''}`,
    method: 'GET',
    options: {
      params: { gender },
    },
    trigger: gender,
    // or
    // trigger: { gender }
    forceDispatchEffect: () => !!gender, // AUTO RUN only if gender is set
  });

  const { data } = response || {};

  const options = [
    { gender: 'female', title: 'Female' },
    { gender: 'male', title: 'Male' },
    { gender: 'unknown', title: 'Unknown' },
  ];

  if (loading) return 'loading...';
  return (
    <div>
      <h2>
        DEMO of
        <span style={{ color: '#F44336' }}>@use-hooks/axios</span>
      </h2>
      {options.map(item => (
        <div key={item.gender}>
          <input
            type="radio"
            id={item.gender}
            value={item.gender}
            checked={gender === item.gender}
            onChange={e => setGender(e.target.value)}
          />
          {item.title}
        </div>
      ))}
      <button type="button" onClick={reFetch}>
        Refresh
      </button>
      <div>
        {error ? (
          error.message || 'error'
        ) : (
          <textarea
            cols="100"
            rows="30"
            defaultValue={JSON.stringify(data || {}, '', 2)}
          />
        )}
      </div>
    </div>
  );
}

```

[Live Show](https://use-hooks.github.io/react-hooks-axios/)

## Development

> Node >= v8 LTS

 - Clone the project to local disk
 - `npm install`
 - `npm start`

## License

MIT

> Generated by [create-react-hooks](https://github.com/use-hooks/create-react-hooks).

 [npm-image]: https://img.shields.io/npm/v/@use-hooks/axios.svg?style=flat-square
 [npm-url]: https://npmjs.org/package/@use-hooks/axios
 [download-image]: https://img.shields.io/npm/dm/@use-hooks/axios.svg?style=flat-square
 [download-url]: https://npmjs.org/package/@use-hooks/axios
 [travis-url]: https://travis-ci.org/use-hooks/react-hooks-axios
 [travis-image]: https://img.shields.io/travis/use-hooks/react-hooks-axios.svg?style=flat-square
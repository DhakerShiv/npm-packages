[![Travis][build-badge]][build]
[![npm package][npm-badge]][npm]

# gatsby-plugin-meta-redirect

Generates client side redirect html files for redirecting on any static file host like s3 or netlify.

It uses window.location.href = url for redirection

## Install

```sh
npm install --save gatsby-plugin-meta-redirect
```

or

```sh
yarn add gatsby-plugin-meta-redirect
```

## How to use

```js
// In your gatsby-config.js
plugins: [
  `gatsby-plugin-client-side-redirect` // keep it in last in list
];
```

### Redirects

You can create redirects using the [`createRedirect`](https://www.gatsbyjs.org/docs/bound-action-creators/#createRedirect) action.

An example:

```js
createRedirect({ fromPath: '/old-url', toPath: '/new-url', isPermanent: true });
```

This will generate the following html files:

### `/old-url/index.html`:

```html
<meta http-equiv="refresh" content="0; URL='/new-url/'" />
```

You can use it using an node api provided by gatsby, for an example
Let's take `createPages`

In you gatsby-node.js file, write following

```
exports.createPages = ({ graphql, actions }) => {
  const {createRedirect} = actions //actions is collection of many actions - https://www.gatsbyjs.org/docs/actions
  createRedirect({ fromPath: '/old-url', toPath: '/new-url', isPermanent: true });
}
```

Above approach is kind of hard code one, let's have a dynamic approach.
Below code is just for understanding and use your schema accordingly

```
exports.createPages = async ({ graphql, actions }) => {
  const {createRedirect} = actions

  // fetch data from a collection which contains list of urls mapping for redirection
  let response = await graphql(`
  query redirects {
    collectionName {
      old_url
      new_url
    }
  }`)

let data = response.data.collectionName
 
/*
 now iterate over data and create redirect for each url
*/
}

```

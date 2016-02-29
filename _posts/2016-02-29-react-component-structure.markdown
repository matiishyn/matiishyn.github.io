---
layout: post
title:  "Presentational and Container Components for React with Redux"
date:   2016-02-29 13:03:48 +0100
categories: programming react redux
---

In this post I'll briefly describe small refactoring of [React] component based on [this article][Dump]

I'm creating an [SPA] using [React] and [Redux] library for [Flux] architecture. 

## MyComponent, old way

![react-component-1]({{ site.url }}assets/2016-02-29-react-component-structure/react-component-1.PNG)

`MyComponent.js`

<pre class='line-numbers'><code class="language-javascript">
import React, { Component, PropTypes } from 'react'
import { connect } from 'react-redux'

import { myAction } from '../actions'     // actions
import styles from './styles.scss'      // CSS Modules

class MyComponent extends Component {
    render() {
        return (<div className={styles.myClass}>MyComponent...</div>)
    }
}

MyComponent.propTypes = {
    propFromUrlParam: PropTypes.string.isRequired,
    propFromState: PropTypes.string.isRequired,
    myAction: PropTypes.func.isRequired
}

function mapStateToProps(state, props) {
    let { propFromUrlParam } = props.params;    // props from URL params
    let { propFromState } = state;              // props from state
    return {propFromUrlParam, propFromState}
}

export default connect(mapStateToProps, {
    // mapping methods to component
    myAction
})(MyComponent)
</code></pre>

This file is going to be really big and one of best practices is to divide it into two logical parts:
*Dump* and *Smart* or *Container* and *Presentational* components. All the explanation you can find [here][Dump]. 
I'll just show how it looks in my case after dividing. So,

## MyComponent, new way

![react-component-2]({{ site.url }}assets/2016-02-29-react-component-structure/react-component-2.PNG)

`MyComponent.js` the [presentational component](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#8ce5):
<pre class='line-numbers'><code class="language-javascript">
import React, { Component, PropTypes } from 'react'
import styles from './styles.scss'  // CSS Modules

class MyComponent extends Component {
    render() {
        return (<div className={styles.myClass}>MyComponent...</div>)
    }
}

MyComponent.propTypes = {
    propFromUrlParam: PropTypes.string.isRequired,
    propFromState: PropTypes.string.isRequired,
    myAction: PropTypes.func.isRequired
}

export default MyComponent
</code></pre>


`MyComponentContainer.js` the [container component](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#c27f):
<pre class='line-numbers'><code class="language-javascript">
import MyComponent from './MyComponent'     // presentational component

import { connect } from 'react-redux'
import { bindActionCreators } from 'redux'
import { myAction } from '../actions'         // actions

function mapStateToProps(state, props) {
    let { propFromUrlParam } = props.params; // prop from URL params
    let { propFromState } = state;
    return {propFromUrlParam, propFromState}
}

function mapDispatchToProps(dispatch) {
    return {
        ...bindActionCreators({
            // mapping methods to component
            myAction
        }, dispatch)
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(MyComponent);
</code></pre>


And a small helper `index.js` which is used just for convenience while importing the component and looks like this:

<pre class='line-numbers'><code class="language-javascript">
export default from './MyComponent' // container components
</code></pre>

Now you may import the `MyComponent` wherever you need it
<pre class='line-numbers'><code class="language-javascript">
import MyComponent from './components/MyComponent' // will import index.js
</code></pre>


[React]: https://facebook.github.io/react
[Redux]: https://github.com/reactjs/redux
[Flux]: https://facebook.github.io/flux/
[SPA]: https://en.wikipedia.org/wiki/Single-page_application
[Dump]: https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0
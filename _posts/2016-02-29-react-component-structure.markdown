---
layout: post
title:  "Component structure for React+Redux"
date:   2016-02-29 13:03:48 +0100
categories: programming react redux
---

In this post I'll briefly describe small logic refactoring of [React] component that can be done.

I'm creating an SPA using [React] and [Redux] library for [Flux] architecture. 
And this is how my component looked before:

`MyComponent.js`:

![react-component-1]({{ site.url }}assets/2016-02-29-react-component-structure/react-component-1.PNG)

<pre class='line-numbers'><code class="language-javascript">
import React, { Component, PropTypes } from 'react'
import { connect } from 'react-redux'

// actions
import { myAction } from '/actions'
import styles from './styles.scss'


class MyComponent extends Component {
    render() {
        return (<div className={styles.myClass}>MyComponent...</div>)
    }
}

MyComponent.propTypes = {
    propFromUrlParam: PropTypes.func.isRequired,
    propFromState: PropTypes.func.isRequired,
    myAction: PropTypes.func.isRequired,
}

function mapStateToProps(state, props) {
    let { propFromUrlParam } = props.params; // prop from URL params
    let { propFromState } = state;
    return {propFromUrlParam, propFromState}
}

export default connect(mapStateToProps, {
    // mapping methods to component
    myAction
})(MyComponent)
</code></pre>


[React]: https://facebook.github.io/react
[Redux]: https://github.com/reactjs/redux
[Flux]: https://facebook.github.io/flux/
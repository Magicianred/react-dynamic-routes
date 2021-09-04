# react-dynamic-routes
An experimental use of routes in react

## Install npm packages

```cmd
npm install react-router-dom
```

## Router types

for better organization of the code I suggest to use a component to keep all the Route (see SiteRoutes.js)

### HashRouter

for single page app, it uses the # in the url

```javascript
import React from 'react';
import { HashRouter as Router, Route, Switch } from 'react-router-dom';

import AboutSection from './AboutSection';
import Home from '../Pages/Home';
import Contact from '../Pages/Contact';
import ContactConfirm from '../Pages/ContactConfirm';
import NotFound from '../Pages/NotFound';
import Error from '../Pages/Error';

function App() {
  return (
    <Fragment>
      <Router>
        <Switch>
            <Route exact path="/" component={Home} />
            <Route path="/about" component={AboutSection} />
            <Route exact path="/contact/confirm" component={ContactConfirm} />
            <Route exact path="/contact" component={Contact} />
            <Route exact path="/error" component={Error} />
            
            <Route  path="*" component={NotFound} />
        </Switch>
      </Router>
    </Fragment>
  );
}

export default App;
```

### BrowserRouter

it uses the HTML5 history API to create components. history can be modified via pushState and replaceState.

```javascript
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

import AboutSection from './AboutSection';
import Home from '../Pages/Home';
import Contact from '../Pages/Contact';
import ContactConfirm from '../Pages/ContactConfirm';
import NotFound from '../Pages/NotFound';
import Error from '../Pages/Error';

function App() {
  return (
    <Fragment>
      <Router>
        <Switch>
            <Route exact path="/" component={Home} />
            <Route path="/about" component={AboutSection} />
            <Route exact path="/contact/confirm" component={ContactConfirm} />
            <Route exact path="/contact" component={Contact} />
            <Route exact path="/error" component={Error} />
            
            <Route  path="*" component={NotFound} />
        </Switch>
      </Router>
    </Fragment>
  );
}

export default App;
```



### SiteRoutes.js (optional)

SiteRoutes.js

```javascript
import React from 'react';
import { Route, Switch } from 'react-router-dom';
import AboutSection from './AboutSection';
import Home from '../Pages/Home';
import Contact from '../Pages/Contact';
import ContactConfirm from '../Pages/ContactConfirm';
import NotFound from '../Pages/NotFound';
import Error from '../Pages/Error';

const SiteRoutes = () =>
    <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={AboutSection} />
        <Route exact path="/contact/confirm" component={ContactConfirm} />
        <Route exact path="/contact" component={Contact} />
        <Route exact path="/error" component={Error} />
        
        <Route  path="*" component={NotFound} />
    </Switch>

export default SiteRoutes;
```

App.js

```javascript
import React from 'react';
import { BrowserRouter as Router } from 'react-router-dom';

function App() {
  return (
    <Router>
        <SiteRoutes />
    </Router>
  );
}

export default App;
```

### SubSection - subroutes (optional)

We can use some component to keep sub section routes

```javascript
import React from 'react';
import { Route, withRouter } from 'react-router-dom';
import About from '../Pages/About';
import WhoAre from '../Pages/WhoAre';
import WhereAre from '../Pages/WhereAre';

const AboutSection = withRouter(props =>
    <>
        <Route exact path={`${props.match.url}`} component={About} />
        <Route exact path={`${props.match.url}/whoare`} component={WhoAre} />
        <Route exact path={`${props.match.url}/whereare`} component={WhereAre} />
    </>
);

export default AboutSection;
```

## Navigate to route

for navigate to route you can create button, a tag or using Link and Redirect component of *react-router-dom*

### Button

```javascript
import React, { Fragment, useCallback } from 'react';
import { useHistory, useRouteMatch } from 'react-router-dom';

const PostList = () => {
    const history = useHistory();
    const match = useRouteMatch();

    const handleDetails = useCallback((id) => {
      history.push(`${match.url}/${id}`);
    }, [history, match]);

    return (
        <Fragment>
            {result.map((item, index) => (
                <div key={index}>
                    <h2>{item.title}</h2>
                    <p>{item.description}</p>
                    <button onClick={() => handleDetails(item.id)}>Go to details</button>
                </div>
            ))}
        </Fragment>
    )
}

export default PostList;
```


### A tag

the *href* attribute of the *a* tag will be the route path configurate in *Route* component

```html
<a href="/about/whoare">Who are</a>
```

### Link component

```javascript
import React, { Fragment, useCallback } from 'react';
import { Link, useHistory, useRouteMatch } from 'react-router-dom';

const PostList = () => {
    const history = useHistory();
    const match = useRouteMatch();

    const handleDetails = useCallback((id) => {
      history.push(`${match.url}/${id}`);
    }, [history, match]);

    return (
        <Fragment>
            {result.map((item, index) => (
                <div key={index}>
                    <h2>{item.title}</h2>
                    <p>{item.description}</p>
                    <Link to={`/blog/${item.id}`}>Go to details</Link>
                </div>
            ))}
        </Fragment>
    )
}

export default PostList;
```

### Redirect component

```javascript
import React, { Fragment } from 'react';
import { Redirect } from 'react-router-dom';

const ReservedArea = () => {

    return (
        <Fragment>
            {!isLogged && (<Redirect to="/" />)}
            <span>Reserved Area</span>
        </Fragment>
    )
}

export default ReservedArea;
```


## References

https://seegatesite.com/tutorial-on-react-router-dom-for-beginners/

https://blog.logrocket.com/react-router-dom-tutorial-examples/
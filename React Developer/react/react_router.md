# React Router

##### Single-Page Apps

- Single-page applications can work in different ways. One way a single-page app loads is by downloading the entire site's contents all at once. This way, when you're navigating around on the site, everything is already available to the browser, and it doesn't need to refresh the page. Another way single-page apps work is by downloading everything that's needed to render the page the user requested. Then when the user navigates to a new page, asynchronous JavaScript requests are made for just the content that was requested.

- Another key factor in a good single-page app is that the URL controls the page content. Single-page applications are highly interactive, and users want to be able to get back to a certain state using just the URL. Why is this important? Bookmarkability! (pretty sure that's not a word...yet) When you bookmark a site, that bookmark is only a URL, it doesn't record the state of that page.

#### React Router

- React Router turns React projects into single-page applications. It does this by providing a number of specialized components that manage the creation of links, manage the app's URL, provide transitions when navigating between different URL locations, and so much more.

  > According to the React Router website:

  > React Router is a collection of navigational components that composes declaratively with your application.

  ```
  npm install --save react-router-dom
  ```

## <BrowserRouter\/>

- going to listen for the changes in url, and make sure correct screen shows up when url changes.

- Here is the code straight from the React Router repository.

  ```js
  class BrowserRouter extends React.Component {
    static propTypes = {
      basename: PropTypes.string,
      forceRefresh: PropTypes.bool,
      getUserConfirmation: PropTypes.func,
      keyLength: PropTypes.number,
      children: PropTypes.node,
    };

    history = createHistory(this.props);

    render() {
      return <Router history={this.history}                  children=this.props.children} />;
    }
  }
  ```

- When you use BrowserRouter, what you're really doing is rendering a Router component and passing it a history prop. Wait, what is history? history comes from the history library (also built by React Training). The whole purpose of this library is it abstracts away the differences in various environments and provides a minimal API that lets you manage the history stack, navigate, confirm navigation, and persist state between sessions.

- So in a nutshell, when you use BrowserRouter, you're creating a history object which will listen to changes in the URL and make sure your app is made aware of those changes.

- Link is a straightforward way to provide declarative, accessible navigation around your application. By passing a to property to the Link component, you tell your app which path to route to.

  ```js
  <Link to="/about">About</Link>
  ```

- If you're experienced with routing on the web, you'll know that sometimes our links need to be a little more complex than just a string. For example, you can pass along query parameters or link to specific parts of a page. What if you wanted to pass state to the new route? To account for these scenarios, instead of passing a string to Links to prop, you can pass it an object like this,

  ```js
  <Link
    to={{
      pathname: "/courses",
      search: "?sort=name",
      hash: "#the-hash",
      state: { fromDashboard: true },
    }}
  >
    Courses
  </Link>
  ```

- with a Route component if you want to be able to pass props to a specific component that the router is going to render, you'll need to use Route’s render prop. As you saw, render puts you in charge of rendering the component which in turn allows you to pass any props to the rendered component as you'd like.

  ```javascript

  <Route to="/" component={Alpha} />
  <Route to="/" render={(props) => <Alpha {...props} />} />

  ```

- In summary, the Route component is a critical piece of building an application with React Router because it's the component which is going to decide which components are rendered based on the current URL path.

---
layout: post
title:      "Relo-Reago: An Esperanto-English Dictionary"
date:       2020-05-22 05:59:25 +0000
permalink:  relo-reago_an_esperanto-english_dictionary
---


Literally meaning "Rail-React", my final project is an English-Esperanto dictionary making use of seed data from [Reta Vortaro](http://reta-vortaro.de).  In addition to providing one-to-one translations between English and Esperanto words, users will also have the ability to log into the site, provide descriptions/clarifications of words and upvote or downvote those descriptions.

Perhaps the biggest challenge I faced in writing this project was wrapping my head around using Redux in an asynchronous manner.  While it seems obvious in retrospect, I wound up spending much of my time figuring out how to get my components to correctly respond to updates in the state triggered by fetch requests called by my Redux actions.  Some lessons I learned on that count are: 

1) Your actions should always return your fetch request, with the last .then statement returning relevant data.

For example, take a simple login action: 
```
export function login({name, password}) {
  return (dispatch) => {
    dispatch({type: 'LOAD_USER'});
    let postObj = API.postObj({name, password});
    let path = API.path('login');
    return fetch(path, postObj)
    .then(r => r.json())
    .then(user => {
      if(user.id) {
        dispatch({type: 'LOGIN', user});
      } else {
        alert("Login failed, try again. \nEnsaluti malsukcesis, provu denove.");
        dispatch({type: 'LOGOUT'});
      }
      return !!user.id;
    })
    .catch(err => {
      dispatch({type: 'LOGOUT'});
      console.log(err);
      alert(err.message);
    });
  }
}
```
There are two `return` statements here: one to return the overall fetch promise, and another within the final promise to return the login status.  That allows us to do this:
```
class Login extends Component {

//...

  handleSubmit = (user) => {
    this.props.login(user)
    .then(isLoggedIn => {
      if(isLoggedIn) {
        this.setState({loggedIn: true})
      }
    })
  }
	//...
}
```
...where `this.props.login` is set to `user => dispatch(login(user))`.  In other words, Redux Thunk isn't doing really any magic in the background that prevents you from simply  returning promises to your components for them to chain off of.

(As a side note, remembering to do this will keep you from thinking you need to dig deep into lifecycle methods that you don't actually need to use.)

2) Have functions you can call inside your `render` method.

To continue with the login example, say you want to redirect the user back to the main page after they log in.  In my case, I looked up a few different ways to do this, and what I finally decided on was to have this method in my class: 
```
  redirectIfLoggedIn = () => {
    if(this.state.loggedIn) {
      return <Redirect to='/' />
    }
		```
	This, then, simply gets called when the page renders, like so: 
	```
	render() {
    return (
      <div>
        {this.redirectIfLoggedIn()}
        <Row>
          <Col s={12}>
            <h2>Login / Ensaluti</h2>
          </Col>
        </Row>
        <UserForm onSubmit={this.handleSubmit} />
      </div>
    );
  }
	```
	Since pages always re-render when the state is updated, our Redirect component will only render when the state changes to indicate a logged in user.  There are perhaps other ways of handling conditional rendering of items, but I've found this to be the cleanest way of making sure that specific items aren't rendered until appropriate.

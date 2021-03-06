# ApiProvider and apiConnect

If you ever used ``react-redux`` before, you've probably have already used ``connect`` and ``<Provider store={yourstore} />``. This library provides the same functionality but for you api object. This way you can provide your api as props to your connected objects.

### How to provide an api
Let's see how we provide our api object so we can connect components to it.

```js
  const api = /* create your api mapper */
  const container = document.getElementById('container');
  
  //Provide your api here
  ReactDOM.render(<ApiProvider api={api}><App /></ApiProvider>, container); 
``` 
### Let's connect
Now that we've provided our api object, we can connect to it. Lets create a ``UserList`` component that will connect to the api to use the ``Users`` resource

```js
class UserList extends Component {
    componentDidMount() {
        //do the api call
        this.props.Users.getUsers();
    }

    render() {
        list = this.props.users.map(user => <li key={user.id}>{user.name}</li>)
        return (
            <ul>
                {list}
            </ul>
        );
    }
}

//Lets connect (we recomend using "compose" from redux and "connect" from react-redux)

const mapStateToProps = (state) => {
    return {
        users : state.users
    };
};

const mapApiToProps = (api) => {
    return {
        Users : api.Users
    };
};

const enchance = compose(
    connect(mapStateToProps),
    apiConnect(mapApiToProps)
);

export default enchance(UserList);
```

And that's it! We've connected our component to the api object. 

### Tips
* If you noticed, we did the api request in the ``componentDidMount`` method. You can read a better explanation for that [here](https://twitter.com/dan_abramov/status/790590733468241920). You should put your api requests in this method.
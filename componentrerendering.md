# Understanding React component re-rendering behavior

Let's go through an example of pure components, stateless functional components and class components to understand how the components re-rendering works.

```
import React from 'react';

class Pure extends React.PureComponent {
  constructor(props) {
    super(props);
    this.state = {
      update: props.toggle,
    };
  }

  render() {
    return (
      <strong>
        <span style={{ color: 'mediumseagreen' }}>Pure: </span>
        {new Date().getSeconds().toString()}
      </strong>
    );
  }
}

const Stateless = props => (
  <strong>
    <span style={{ color: 'orange' }}>Stateless: </span>
    {new Date().getSeconds().toString()}
  </strong>
);

class Normal1 extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      update: props.toggle,
    };
  }

  render() {
    return (
      <strong>
        <span style={{ color: 'dodgerblue' }}>Normal1: </span>
        {new Date().getSeconds().toString()}
      </strong>
    );
  }
}

class Normal2 extends React.Component {
  state = {
    update: this.props.toggle,
  };

  render() {
    return (
      <strong>
        <span style={{ color: 'green' }}>Normal2: </span>
        {new Date().getSeconds().toString()}
      </strong>
    );
  }
}

class Normal3 extends React.Component {
  render() {
    return (
      <strong>
        <span style={{ color: 'red' }}>Normal3: </span>
        {new Date().getSeconds().toString()}
      </strong>
    );
  }
}

class App extends React.Component {
  state = { toggle: true };

  componentDidMount() {
    setInterval(() => {
      this.setState({ toggle: true });
      // this.setState({ toggle: !this.state.toggle })
    }, 1000);
  }

  render() {
    const { toggle } = this.state;
    return (
      <div>
        <Pure toggle={toggle} />
        <br />
        <Stateless toggle={toggle} />
        <br />
        <Normal1 toggle={toggle} />
        <br />
        <Normal2 toggle={toggle} />
        <br />
        <Normal3 />
        <br />
      </div>
    );
  }
}

export default App;
```

If you run the above code, you'll notice that all components re-render every second except Pure Component. Pure component does not re-render with this code because PureComponent changes the life-cycle method shouldComponentUpdate and adds some logic to automatically check whether a re-render is required for the component. This allows a PureComponent to call the method render only if it detects changes in state or props. In this case, no change is happening in the state so its not re-rendering.

**Why is the Stateless component re-rendering when the state is not actually changing?**

 If setState() is called in the component or one of its parents, stateless components always re-render. This is because stateless functional components don't have a shouldComponentUpdate(). Since React 16.6, you can use [React.memo](https://reactjs.org/docs/react-api.html#reactmemo) for functional components to prevent re-render, similarly to PureComponent for class components. Change this line

```
 const Stateless = props => (
```

to
```
const StateLess = React.memo((props) => {
```

**Why are other components being rerendered when state is not changing?**

That's because by default every other component does re-render unless they implement shouldComponentUpdate. Adding even a blank shouldUpdateComponent will prevent them from re-rendering because if you override it with an empty function, it will return undefined, which is cast to false, therefore your component never re-renders (except in the case where a forceUpdate will force it to render without checking shouldComponentUpdate)

**What will happen if we uncomment line ```this.setState({ toggle: !this.state.toggle })``` in App:componentDidMount()?**

This will actually mutate the state every 1 second, hence re-rendering every single component on the page.

*Code Credits: Udacity*
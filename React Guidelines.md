#React Guidelines#

##Object Instanciation/Creation##
var reactComponentName = react.CreateClass();

##Components Properties and State##
Each component can have a set of props and a state.
The properties(props) are passed from a parent node, defining it in the call of the component.
The state is defined firsly in the getInitialState method, and can be changed as many times as you wish.
Generally, the state should be in the parents nodes, in order to be accessible in the child components.

##Component Lyfecycle##
React component have 4 different cycles, depending on what's happening to the component. The principal, is the initialization lifecycle where the component is instanciated.

####Initialization:####
* getDefaultProps
* getInitialState
* componentWillMount, before rendering the component, it's a good option to retrieve information from the server.
* render, where the component is rendered into the DOM.
* componentDidMount, after rendering the component can be manipulated by the DOM.

####State Changes:####
* shouldComponentUpdate
* componentWillUpdate
* Render
* componentDidUpdate

####Props Changes:####
* componentWillReceiveProps
* shouldComponentUpdate
* componentWillUpdate
* Render
* componentDidUpdate

####Unmounting:####
* componentWillUnmount 

##Dynamic Data##
When manipulating dynamic data, for example in a table or list, the table rows or li's need to be identified by the key property.
Simply by:
```javascript
* <tr key={author.id}>
* <li key={course.id}>
```

##Displaying Arrays of Data##
To iterate over an array of information, use the map method, where it's possible to access each object information.
```javascript
this.state.courses.map(function(course) {
	return (<tr key={course.id} >
		<td>{course.name}</td>
		<td><Link to={'/Courses/CourseDetails/' + course.id } className="link-details">See Details</Link></td>
		</tr>);
}
```

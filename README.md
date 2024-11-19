# LifeCycleMethodsClassComponent


import React from 'react';

// Example of a class component showcasing all lifecycle methods
class LifecycleComponent extends React.Component {
    // 1. Mounting Phase Methods
    constructor(props) {
        super(props);
        this.state = {
            data: [],
            loading: true,
            error: null
        };
        console.log('1. Constructor called');
        // Use constructor for:
        // - Initializing state
        // - Binding event handlers
        // - Setting up initial configuration
    }

    static getDerivedStateFromProps(props, state) {
        console.log('2. getDerivedStateFromProps called');
        // Use for:
        // - Updating state based on props changes
        // - Called on both mount and update
        // Must return new state or null
        return null;
    }

    componentDidMount() {
        console.log('4. ComponentDidMount called');
        // Use for:
        // - API calls
        // - Subscriptions 
        // - DOM manipulations
        // - Third-party library integrations
        
        // Example API call
        fetch('https://api.example.com/data')
            .then(response => response.json())
            .then(data => this.setState({ data, loading: false }))
            .catch(error => this.setState({ error, loading: false }));
    }

    // 2. Updating Phase Methods
    shouldComponentUpdate(nextProps, nextState) {
        console.log('5. shouldComponentUpdate called');
        // Use for:
        // - Performance optimization
        // - Control if component should re-render
        // Return true to re-render, false to prevent update
        return true;
    }

    getSnapshotBeforeUpdate(prevProps, prevState) {
        console.log('7. getSnapshotBeforeUpdate called');
        // Use for:
        // - Capture DOM information before update
        // - Pass value to componentDidUpdate
        // - e.g., scroll position
        return null;
    }

    componentDidUpdate(prevProps, prevState, snapshot) {
        console.log('8. componentDidUpdate called');
        // Use for:
        // - DOM updates after component updates
        // - Network requests based on prop changes
        // - Don't update state without conditions!
        
        // Example of conditional state update
        if (prevProps.userId !== this.props.userId) {
            this.setState({ loading: true });
            fetch(`https://api.example.com/users/${this.props.userId}`)
                .then(response => response.json())
                .then(data => this.setState({ data, loading: false }))
                .catch(error => this.setState({ error, loading: false }));
        }
    }

    // 3. Unmounting Phase Method
    componentWillUnmount() {
        console.log('9. componentWillUnmount called');
        // Use for:
        // - Cleanup tasks
        // - Cancel network requests
        // - Clean up subscriptions
        // - Remove event listeners
        
        // Example cleanup
        if (this.subscription) {
            this.subscription.unsubscribe();
        }
    }

    // 4. Error Handling Methods
    static getDerivedStateFromError(error) {
        console.log('Error boundary: getDerivedStateFromError');
        // Use for:
        // - Update state in response to error
        // - Render fallback UI
        return { hasError: true, error };
    }

    componentDidCatch(error, errorInfo) {
        console.log('Error boundary: componentDidCatch');
        // Use for:
        // - Log errors
        // - Send error reports
        // - Display fallback UI
        console.error('Error caught:', error);
        console.error('Error info:', errorInfo);
    }

    // Required render method
    render() {
        console.log('3/6. Render called');
        // Use for:
        // - Return JSX to display
        // - Should be pure function
        // - Don't modify state here

        const { loading, error, data, hasError } = this.state;

        if (hasError) {
            return <div>Something went wrong. Please try again later.</div>;
        }

        if (loading) {
            return <div>Loading...</div>;
        }

        if (error) {
            return <div>Error: {error.message}</div>;
        }

        return (
            <div>
                <h1>Lifecycle Component</h1>
                <pre>{JSON.stringify(data, null, 2)}</pre>
            </div>
        );
    }
}

// Example usage:
class App extends React.Component {
    render() {
        return (
            <div>
                <h1>React Lifecycle Methods Demo</h1>
                <LifecycleComponent userId="123" />
            </div>
        );
    }
}

export default App;

/* 
Lifecycle Method Execution Order:

On Mount:
1. constructor()
2. getDerivedStateFromProps()
3. render()
4. componentDidMount()

On Update:
5. getDerivedStateFromProps()
6. shouldComponentUpdate()
7. render()
8. getSnapshotBeforeUpdate()
9. componentDidUpdate()

On Unmount:
10. componentWillUnmount()

On Error:
- getDerivedStateFromError()
- componentDidCatch()
*/

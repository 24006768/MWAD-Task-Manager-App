# MWAD-Task-Manager-App
# AIM

To develop a Task Manager Application where users can add tasks, mark tasks as completed, delete tasks, and filter tasks based on their status. The application demonstrates the use of React hooks for efficient state management and performance optimization.

# PROCEDURE:

* Set up the React project using a React app creation tool.
* Implement task management features:
    * Add new tasks
    * Mark tasks as completed/uncompleted
    * Delete tasks
* Filter tasks by their status (all, completed, uncompleted)
    * Use React hooks for efficient performance:
    * useReducer to manage tasks
    * useRef to focus the input box automatically
    * useMemo to optimize task filtering
    * useCallback to optimize event handlers
* Style the application for clarity and usability.
* Run and test the application to ensure all functionalities work correctly.   
* Upload the project to GitHub by initializing git, committing the code, and pushing to a remote repository.

# PROGRAM: 

# App.jsx:
```
import React, { useReducer, useRef, useMemo, useCallback, useState } from "react";
import "./App.css";

// Reducer function to manage tasks
const taskReducer = (tasks, action) => {
  switch (action.type) {
    case "ADD":
      return [...tasks, { id: Date.now(), text: action.payload, completed: false }];
    case "TOGGLE":
      return tasks.map(task =>
        task.id === action.payload ? { ...task, completed: !task.completed } : task
      );
    case "DELETE":
      return tasks.filter(task => task.id !== action.payload);
    default:
      return tasks;
  }
};

function App() {
  const [tasks, dispatch] = useReducer(taskReducer, []);
  const [filter, setFilter] = useState("all"); // all, completed, uncompleted
  const inputRef = useRef();

  // Memoized filtered tasks
  const filteredTasks = useMemo(() => {
    switch (filter) {
      case "completed":
        return tasks.filter(task => task.completed);
      case "uncompleted":
        return tasks.filter(task => !task.completed);
      default:
        return tasks;
    }
  }, [tasks, filter]);

  // Memoized handlers
  const handleAdd = useCallback(() => {
    const text = inputRef.current.value.trim();
    if (text) {
      dispatch({ type: "ADD", payload: text });
      inputRef.current.value = "";
      inputRef.current.focus();
    }
  }, []);

  const handleToggle = useCallback(id => {
    dispatch({ type: "TOGGLE", payload: id });
  }, []);

  const handleDelete = useCallback(id => {
    dispatch({ type: "DELETE", payload: id });
  }, []);

  return (
    <div className="App">
      <h1>Task Manager</h1>
      <div className="input-section">
        <input ref={inputRef} type="text" placeholder="Enter a task" />
        <button onClick={handleAdd}>Add Task</button>
      </div>

      <div className="filter-section">
        <button onClick={() => setFilter("all")}>All</button>
        <button onClick={() => setFilter("completed")}>Completed</button>
        <button onClick={() => setFilter("uncompleted")}>Uncompleted</button>
      </div>

      <ul>
        {filteredTasks.map(task => (
          <li key={task.id} className={task.completed ? "completed" : ""}>
            <span onClick={() => handleToggle(task.id)}>{task.text}</span>
            <button onClick={() => handleDelete(task.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```
# App.css:
```
.App {
  font-family: Arial, sans-serif;
  max-width: 600px;
  margin: auto;
  text-align: center;
  padding: 2rem;
}

input {
  padding: 0.5rem;
  width: 60%;
  margin-right: 0.5rem;
}

button {
  padding: 0.5rem 1rem;
  margin: 0.2rem;
}

.completed {
  text-decoration: line-through;
  color: gray;
  cursor: pointer;
}

li {
  list-style: none;
  display: flex;
  justify-content: space-between;
  margin: 0.5rem 0;
  border-bottom: 1px solid #ccc;
  padding-bottom: 0.3rem;
  cursor: pointer;
}
```
# OUTPUT:

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/5ce4e470-38a7-4a81-8d0e-722130daed70" />

# RESULT:

The Task Manager app allows users to add, complete, delete, and filter tasks efficiently with smooth performance.

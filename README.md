server.js 

const express = require('express');
const cors = require('cors');
const mongoose = require('./db');
const employeeRoutes = require('./routes/employeeRoutes');

const app = express();

app.use(cors());
app.use(express.json());

// Mount routes
app.use('/api/employees', employeeRoutes);

// Start server
const PORT = 8080;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});

-------------------------------------------------------
db.js

const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/employee_db');

const db = mongoose.connection;

db.on('error', (err) => console.error('MongoDB connection error:', err));
db.once('open', () => console.log('Connected to MongoDB'));

module.exports = mongoose;

---------------------------------------------------------

models->employee.js

const mongoose = require('mongoose');

const employeeSchema = new mongoose.Schema({
  name: { type: String, required: true },
  department: { type: String, required: true },
  salary: { type: Number, required: true },
});

module.exports = mongoose.model('Employee', employeeSchema);

-----------------------------------------------------------

routes -> employeeRoutes.js

const express = require('express');
const router = express.Router();
const Employee = require('../models/employee');

// Create
router.post('/', async (req, res) => {
  try {
    const newEmp = new Employee(req.body);
    const savedEmp = await newEmp.save();
    res.status(201).json(savedEmp);
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
});

// Read All
router.get('/', async (req, res) => {
  try {
    const employees = await Employee.find();
    res.json(employees);
  } catch (err) {
    res.status(500).json({ error: err.message });
  }
});

// Read One
router.get('/:id', async (req, res) => {
  try {
    const emp = await Employee.findById(req.params.id);
    res.json(emp);
  } catch (err) {
    res.status(404).json({ error: 'Employee not found' });
  }
});

// Update
router.put('/:id', async (req, res) => {
  try {
    const updated = await Employee.findByIdAndUpdate(req.params.id, req.body, { new: true });
    res.json(updated);
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
});

// Delete
router.delete('/:id', async (req, res) => {
  try {
    await Employee.findByIdAndDelete(req.params.id);
    res.json({ message: 'Employee deleted' });
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
});

module.exports = router;


----------------------------------------------------------------------------------------------
react ui component


import React, { useState, useEffect } from 'react';
import axios from 'axios';

const Employee = () => {
  const [employees, setEmployees] = useState([]);
  const [newEmployee, setNewEmployee] = useState({ name: '', department: '', salary: '' });
  const [editableEmployee, setEditableEmployee] = useState(null);

  // Fetch all employees
  useEffect(() => {
    axios.get('http://localhost:8080/api/employees')
      .then(res => setEmployees(res.data))
      .catch(err => console.log('Error:', err));
  }, []);

  // Handle form submission to create a new employee
  const handleSubmit = (e) => {
    e.preventDefault();
    if (editableEmployee) {
      // Update the employee
      axios.put(`http://localhost:8080/api/employees/${editableEmployee._id}`, newEmployee)
        .then(res => {
          setEmployees(employees.map(emp => (emp._id === editableEmployee._id ? res.data : emp)));
          setNewEmployee({ name: '', department: '', salary: '' });
          setEditableEmployee(null); // Reset editable state
        })
        .catch(err => console.log('Error:', err));
    } else {
      // Create a new employee
      axios.post('http://localhost:8080/api/employees', newEmployee)
        .then(res => {
          setEmployees([...employees, res.data]);
          setNewEmployee({ name: '', department: '', salary: '' });
        })
        .catch(err => console.log('Error:', err));
    }
  };

  // Handle delete employee
  const handleDelete = (id) => {
    axios.delete(`http://localhost:8080/api/employees/${id}`)
      .then(() => {
        setEmployees(employees.filter(emp => emp._id !== id));
      })
      .catch(err => console.log('Error:', err));
  };

  // Handle edit employee
  const handleEdit = (employee) => {
    setEditableEmployee(employee);
    setNewEmployee({ name: employee.name, department: employee.department, salary: employee.salary });
  };

  return (
    <div>
      <h1>Employee Management</h1>
      
      {/* Add or Update Employee Form */}
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          placeholder="Name"
          value={newEmployee.name}
          onChange={(e) => setNewEmployee({ ...newEmployee, name: e.target.value })}
        />
        <input
          type="text"
          placeholder="Department"
          value={newEmployee.department}
          onChange={(e) => setNewEmployee({ ...newEmployee, department: e.target.value })}
        />
        <input
          type="number"
          placeholder="Salary"
          value={newEmployee.salary}
          onChange={(e) => setNewEmployee({ ...newEmployee, salary: e.target.value })}
        />
        <button type="submit">{editableEmployee ? 'Update Employee' : 'Add Employee'}</button>
      </form>

      {/* List of Employees */}
      <ul>
        {employees.map(emp => (
          <li key={emp._id}>
            {emp.name} - {emp.department} - ${emp.salary}
            <button onClick={() => handleEdit(emp)}>Edit</button>
            <button onClick={() => handleDelete(emp._id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default Employee;


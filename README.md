import React, { useState, useEffect } from "react";

function EmployeeForm({ addOrUpdateEmployee, editEmployee }) {
  const initialState = {
    employeeId: "",
    name: "",
    email: "",
    phone: "",
    department: "",
    designation: "",
    salary: "",
    joiningDate: "",
  };

  const [formData, setFormData] = useState(initialState);

  useEffect(() => {
    if (editEmployee) {
      setFormData(editEmployee);
    }
  }, [editEmployee]);

  const validate = () => {
    if (
      !formData.employeeId ||
      !formData.name ||
      !formData.email ||
      !formData.phone ||
      !formData.department ||
      !formData.designation ||
      !formData.salary ||
      !formData.joiningDate
    ) {
      alert("All fields are required");
      return false;
    }

    const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailPattern.test(formData.email)) {
      alert("Invalid email format");
      return false;
    }

    if (isNaN(formData.salary)) {
      alert("Salary must be numeric");
      return false;
    }

    return true;
  };

  const handleChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value,
    });
  };

  const handleSubmit = (e) => {
    e.preventDefault();

    if (!validate()) return;

    addOrUpdateEmployee(formData);
    setFormData(initialState);
  };

  return (
    <form className="employee-form" onSubmit={handleSubmit}>
      {Object.keys(formData).map((key) => (
        <input
          key={key}
          type={key === "joiningDate" ? "date" : "text"}
          name={key}
          placeholder={key}
          value={formData[key]}
          onChange={handleChange}
        />
      ))}

      <button type="submit">
        {editEmployee ? "Update Employee" : "Add Employee"}
      </button>
    </form>
  );
}

export default EmployeeForm;

# Ex06 BMI Calculator
## Date: 02/09/2026


## AIM
To develop a responsive and interactive Body Mass Index (BMI) Calculator using React that allows users to input their height and weight, and calculates their BMI to categorize their health status (e.g., Underweight, Normal, Overweight, Obese).

## DESIGN STEPS

### STEP 1: Initialize React Project

<li>Create a new React app using create-react-app.</li>
<li>Install React Router using:</li>
npm install react-router-dom

### STEP 2: Set Up Routing

Create routing structure with react-router-dom:

<li>Home route (/) – Intro or Navigation</li>

<li>BMI Calculator route (/bmi)</li>

<li>Result route (/result)</li>

### STEP 3: Design the BMI Form Page

<li>Create a form to accept Height (in cm or m) and Weight (in kg).</li>

<li>On form submit, navigate to the result page with entered values via URL query params or context/state.</li>

## STEP 4: Handle Input Validation

<li>Check if height and weight are valid numbers.</li>

<li>Optionally, show error messages for invalid inputs.</li>

### STEP 5: Perform BMI Calculation

<li>In the result component:

<li>Extract height and weight from the route (URL or passed state).</li>

<li>Apply the BMI formula:</li>

![image](https://github.com/user-attachments/assets/ec785506-c96b-489e-8783-fb1a5d36101a)
​
 
<li>Convert height from cm to m if needed.</li></li>

### STEP 6: Display Result

<li>Show calculated BMI.</li>

<li>Show category based on BMI range:

<li>Underweight, Normal, Overweight, Obese, etc.</li></li>

### STEP 7: Navigation Options

<li>Provide a button to go back to the BMI form to calculate again.</li>

### STEP 8: Enhancements

<li>Add styling using CSS or Tailwind.</li>

## PROGRAM
BmiForm.js
```j
import React, { useState } from "react";
import { useNavigate } from "react-router-dom";

function BmiForm() {
  const [height, setHeight] = useState("");
  const [weight, setWeight] = useState("");
  const [error, setError] = useState("");
  const navigate = useNavigate();

  const handleSubmit = (e) => {
    e.preventDefault();

    const h = parseFloat(height);
    const w = parseFloat(weight);

    if (!height || !weight) {
      setError("Please fill in both height and weight.");
      return;
    }
    if (isNaN(h) || isNaN(w)) {
      setError("Height and weight must be valid numbers.");
      return;
    }
    if (h <= 0 || w <= 0) {
      setError("Height and weight must be greater than zero.");
      return;
    }
    if (h > 300) {
      setError("Please enter height in cm (e.g. 170), not meters.");
      return;
    }

    setError("");
    navigate(`/result?height=${h}&weight=${w}`);
  };

  return (
    <div className="card">
      <h2>Enter Your Details</h2>
      <form onSubmit={handleSubmit} className="bmi-form">
        <label htmlFor="height">Height (cm)</label>
        <input
          id="height"
          type="number"
          step="0.1"
          placeholder="e.g. 170"
          value={height}
          onChange={(e) => setHeight(e.target.value)}
        />

        <label htmlFor="weight">Weight (kg)</label>
        <input
          id="weight"
          type="number"
          step="0.1"
          placeholder="e.g. 65"
          value={weight}
          onChange={(e) => setWeight(e.target.value)}
        />

        {error && <p className="error-text">{error}</p>}

        <button type="submit" className="btn">
          Calculate BMI
        </button>
      </form>
    </div>
  );
}

export default BmiForm;
```
Home.js
```j
import React from "react";
import { Link } from "react-router-dom";

function Home() {
  return (
    <div className="card">
      <h2>Know Your BMI</h2>
      <p>
        Body Mass Index (BMI) is a simple measure that uses your height and
        weight to estimate whether your weight is healthy. Click below to
        calculate yours.
      </p>
      <Link to="/bmi" className="btn">
        Calculate My BMI
      </Link>
    </div>
  );
}

export default Home;
```
Result.js
```j
import React from "react";
import { useNavigate, useSearchParams } from "react-router-dom";

function getCategory(bmi) {
  if (bmi < 18.5) return { label: "Underweight", className: "cat-under" };
  if (bmi < 25) return { label: "Normal", className: "cat-normal" };
  if (bmi < 30) return { label: "Overweight", className: "cat-over" };
  return { label: "Obese", className: "cat-obese" };
}

function Result() {
  const [searchParams] = useSearchParams();
  const navigate = useNavigate();

  const heightCm = parseFloat(searchParams.get("height"));
  const weight = parseFloat(searchParams.get("weight"));

  if (!heightCm || !weight) {
    return (
      <div className="card">
        <h2>No data found</h2>
        <p>Please go back and enter your height and weight.</p>
        <button className="btn" onClick={() => navigate("/bmi")}>
          Go to BMI Form
        </button>
      </div>
    );
  }

  const heightM = heightCm / 100;
  const bmi = weight / (heightM * heightM);
  const bmiRounded = bmi.toFixed(2);
  const { label, className } = getCategory(bmi);

  return (
    <div className="card">
      <h2>Your Result</h2>
      <p className="bmi-value">{bmiRounded}</p>
      <p className={`bmi-category ${className}`}>{label}</p>

      <div className="bmi-details">
        <p>Height: {heightCm} cm</p>
        <p>Weight: {weight} kg</p>
      </div>

      <button className="btn" onClick={() => navigate("/bmi")}>
        Calculate Again
      </button>
    </div>
  );
}

export default Result;
```

## OUTPUT
<img width="1920" height="1080" alt="Screenshot (414)" src="https://github.com/user-attachments/assets/abaa5494-080d-49f7-8d5f-6736d85d334d" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a8e39378-03d2-446c-82ec-0f6a23f6cf6b" />

## RESULT
The BMI Calculator successfully takes user input for height and weight, performs the BMI calculation in real-time using React state and event handling, and displays the BMI value along with the corresponding health category.

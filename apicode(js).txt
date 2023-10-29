const express = require("express");
const app = express();
const bodyParser = require("body-parser");
const jpdb = require("jsonpowerdb");

app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

app.use(express.static(__dirname));

// Connect to your JsonPowerDB instance
const db = new jpdb.JsonPowerDB("your_database_directory");

// Implement a route to save a new student record
app.post("/api/saveStudent", (req, res) => {
  const data = req.body;

  // Check if the Roll No (primary key) exists in the database
  if (db.get("STUDENT-TABLE", data.RollNo)) {
    // Roll No already exists, return an error
    return res.status(400).json({ error: "Roll No already exists" });
  }

  // If Roll No doesn't exist, save the student data
  db.insert("STUDENT-TABLE", data);
  db.save();

  res.status(200).json({ message: "Student record saved successfully" });
});

// Implement a route to update an existing student record
app.put("/api/updateStudent/:rollNo", (req, res) => {
  const rollNo = req.params.rollNo;
  const data = req.body;

  // Check if the Roll No exists in the database
  if (!db.get("STUDENT-TABLE", rollNo)) {
    // Roll No doesn't exist, return an error
    return res.status(404).json({ error: "Student not found" });
  }

  // Update the student data
  db.update("STUDENT-TABLE", rollNo, data);
  db.save();

  res.status(200).json({ message: "Student record updated successfully" });
});

app.listen(3000, () => {
  console.log("Server is running on port 3000");
});




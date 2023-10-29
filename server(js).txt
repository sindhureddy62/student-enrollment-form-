const express = require("express");
const app = express();
const bodyParser = require("body-parser");

app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

app.use(express.static(__dirname));

// Implement routes for saving and updating data in JsonPowerDB here

app.listen(3000, () => {
    console.log("Server is running on port 3000");
});



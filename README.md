# Hand Gesture Mouse Control

This project implements a real-time hand gesture-based mouse control system using computer vision and machine learning.

## Project Structure

```
HandGestureMouseControl/
├── data/
│   ├── raw/
│   │   └── LeapGestRecog/                  # Unzipped dataset directory
│   └── processed/
│       └── gesture_images/                 # (Optional) Converted or cleaned images
├── src/
│   ├── data_preparation.py                 # Script to load, clean, preprocess dataset
│   ├── hand_tracking.py                    # Real-time hand tracking module (MediaPipe)
│   ├── gesture_control.py                  # Gesture recognition logic using dataset/model
│   ├── mouse_controller.py                 # Control mouse actions via gestures
│   ├── utils.py                             # Helper functions (mapping coords, smoothing, etc)
│   └── main.py                              # Main entry: train model (optional) + run real-time system
├── models/
│   └── gesture_recognition_model.h5         # (If you train a model) saved model file
├── notebooks/
│   └── dataset_exploration.ipynb            # Notebook to explore dataset (visualise, stats)
├── requirements.txt
├── README.md
├── project_report.docx
├── flowchart.png
└── .gitignore
```

## Setup

1. Clone the repository and navigate to the project directory.
2. Install dependencies: `pip install -r requirements.txt`
3. Download the LeapGestRecog dataset and place it in `data/raw/LeapGestRecog/`.
4. (Optional) Train the model using `python src/data_preparation.py` and save to `models/`.
5. Run the real-time system: `python src/main.py`

## Usage

- Run `python src/main.py` to start the hand gesture mouse control.
- Use gestures like fist (click), peace (scroll up), thumbs up (scroll down), point (move cursor).

## Dependencies

- OpenCV
- MediaPipe
- TensorFlow
- PyAutoGUI
- NumPy
- Scikit-learn
- Matplotlib
- Jupyter

## License

This project is licensed under the MIT License.















































I) DOCKER



1.Form Validation code in react

structure:

JavaFormValidationApp/
├── src/
│   ├── FormServer.java
├── Dockerfile
└── README.md



1)BMIServer.java

import java.io.*;
import java.net.*;
import java.util.regex.*;

public class FormServer {
    public static void main(String[] args) throws IOException {
        int port = 9091;
        ServerSocket serverSocket = new ServerSocket(port);
        System.out.println("Form Validation Server running on http://localhost:" + port);

        while (true) {
            Socket socket = serverSocket.accept();
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            BufferedWriter out = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));

            String requestLine;
            String query = "";
            while ((requestLine = in.readLine()) != null && !requestLine.isEmpty()) {
                if (requestLine.startsWith("GET")) {
                    int qIndex = requestLine.indexOf("?");
                    if (qIndex != -1) {
                        int end = requestLine.indexOf(" ", qIndex);
                        query = requestLine.substring(qIndex + 1, end);
                    }
                }
            }

            String name = "", email = "", msg = "";
            if (!query.isEmpty()) {
                String[] params = query.split("&");
                for (String p : params) {
                    String[] kv = p.split("=");
                    if (kv[0].equals("name")) name = kv[1].replace("+", " ");
                    if (kv[0].equals("email")) email = kv[1];
                }

                Pattern emailPattern = Pattern.compile("^[\\w.-]+@[\\w.-]+\\.\\w+$");
                if (name.isEmpty() || email.isEmpty()) msg = "All fields are required!";
                else if (!emailPattern.matcher(email).matches()) msg = "Invalid email address!";
                else msg = "Form submitted successfully!";
            }

            String html = """
                <html>
                <head><title>Form Validation</title></head>
                <body style="font-family:Arial;text-align:center;">
                    <h1>Form Validation</h1>
                    <form method="GET" action="/">
                        <label>Name:</label><input type="text" name="name"><br><br>
                        <label>Email:</label><input type="text" name="email"><br><br>
                        <input type="submit" value="Submit">
                    </form>
            """;

            if (!msg.isEmpty()) html += "<h2>" + msg + "</h2>";

            html += "</body></html>";

            out.write("HTTP/1.1 200 OK\r\n");
            out.write("Content-Type: text/html\r\n");
            out.write("Content-Length: " + html.length() + "\r\n\r\n");
            out.write(html);
            out.flush();
            socket.close();
        }
    }
}


2)Dockerfile

FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY build/FormServer.class /app/
EXPOSE 9091
CMD ["java", "FormServer"]



commands to run

javac src\FormServer.java -d build
docker build -t java-form-server .
docker run -p 9091:9091 java-form-server



2.BMI calculator or currency converter in Java

structure:

JavaBMIApp/
├── out/
│  
├── src/
│   ├── BMIServer.java
 
├── Dockerfile
└── README.md


and codes are

1)BMIServer.java

import java.io.*;
import java.net.*;

public class BMIServer {
    public static void main(String[] args) throws IOException {
        int port = 9090;
        ServerSocket serverSocket = new ServerSocket(port);
        System.out.println("BMI Server running on http://localhost:" + port);

        while (true) {
            Socket clientSocket = serverSocket.accept();
            BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
            BufferedWriter out = new BufferedWriter(new OutputStreamWriter(clientSocket.getOutputStream()));

            String requestLine;
            String query = "";
            while ((requestLine = in.readLine()) != null && !requestLine.isEmpty()) {
                if (requestLine.startsWith("GET")) {
                    int qIndex = requestLine.indexOf("?");
                    if (qIndex != -1) {
                        int end = requestLine.indexOf(" ", qIndex);
                        query = requestLine.substring(qIndex + 1, end);
                    }
                }
            }

            double bmi = 0;
            String result = "";

            if (!query.isEmpty()) {
                String[] params = query.split("&");
                double w = 0, h = 0;
                for (String p : params) {
                    String[] kv = p.split("=");
                    if (kv[0].equals("w")) w = Double.parseDouble(kv[1]);
                    if (kv[0].equals("h")) h = Double.parseDouble(kv[1]);
                }
                if (h > 0) bmi = w / (h * h);
                if (bmi < 18.5) result = "Underweight";
                else if (bmi < 25) result = "Normal";
                else if (bmi < 30) result = "Overweight";
                else result = "Obese";
            }

            String html = """
                <html>
                <head><title>BMI Calculator</title></head>
                <body style="font-family:Arial;text-align:center;">
                    <h1>BMI Calculator</h1>
                    <form method="GET" action="/">
                        <label>Weight (kg):</label><input type="text" name="w"><br><br>
                        <label>Height (m):</label><input type="text" name="h"><br><br>
                        <input type="submit" value="Calculate">
                    </form>
            """;

            if (bmi > 0)
                html += "<h2>Your BMI: " + String.format("%.2f", bmi) + " (" + result + ")</h2>";

            html += "</body></html>";

            out.write("HTTP/1.1 200 OK\r\n");
            out.write("Content-Type: text/html\r\n");
            out.write("Content-Length: " + html.length() + "\r\n");
            out.write("\r\n");
            out.write(html);
            out.flush();

            clientSocket.close();
        }
    }
}


2)Dockerfile

# Use lightweight Java Runtime (JRE only)
FROM eclipse-temurin:21-jre-alpine

# Set working directory
WORKDIR /app

# Copy precompiled class file
COPY build/BMIServer.class /app/

# Expose port 9090
EXPOSE 9090

# Run the server
CMD ["java", "BMIServer"]


commands to run

1) javac src\BMIServer.java -d build
(O/P) E:\Python\JavaBMIApp\build\BMIServer.class
2) docker build -t java-bmi-server .
3)docker run -p 9090:9090 java-bmi-server
(O/P)LocalHost



3.Simple CRUD application (locally or globally)

structure:

JavaCRUDApp/
├── src/
│   ├── CRUDServer.java
├── Dockerfile
└── README.md




1)BMIServer.java

import java.io.*;
import java.net.*;
import java.util.*;

public class CRUDServer {
    private static Map<Integer, String> data = new HashMap<>();
    private static int idCounter = 1;

    public static void main(String[] args) throws IOException {
        int port = 9092;
        ServerSocket serverSocket = new ServerSocket(port);
        System.out.println("CRUD Server running on http://localhost:" + port);

        while (true) {
            Socket socket = serverSocket.accept();
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            BufferedWriter out = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));

            String requestLine;
            String query = "";
            while ((requestLine = in.readLine()) != null && !requestLine.isEmpty()) {
                if (requestLine.startsWith("GET")) {
                    int qIndex = requestLine.indexOf("?");
                    if (qIndex != -1) {
                        int end = requestLine.indexOf(" ", qIndex);
                        query = requestLine.substring(qIndex + 1, end);
                    }
                }
            }

            String msg = "";
            if (!query.isEmpty()) {
                String[] params = query.split("&");
                String action = "", name = "";
                int id = 0;

                for (String p : params) {
                    String[] kv = p.split("=");
                    if (kv[0].equals("action")) action = kv[1];
                    if (kv[0].equals("name")) name = kv[1].replace("+", " ");
                    if (kv[0].equals("id")) id = Integer.parseInt(kv[1]);
                }

                switch (action) {
                    case "add" -> {
                        data.put(idCounter++, name);
                        msg = "Added Successfully!";
                    }
                    case "delete" -> {
                        data.remove(id);
                        msg = "Deleted Successfully!";
                    }
                }
            }

            StringBuilder table = new StringBuilder("<table border='1' style='margin:auto'><tr><th>ID</th><th>Name</th></tr>");
            for (var e : data.entrySet()) {
                table.append("<tr><td>").append(e.getKey()).append("</td><td>").append(e.getValue()).append("</td></tr>");
            }
            table.append("</table>");

            String html = """
                <html>
                <head><title>Simple CRUD App</title></head>
                <body style="font-family:Arial;text-align:center;">
                    <h1>CRUD Application</h1>
                    <form method="GET" action="/">
                        <input type="hidden" name="action" value="add">
                        <label>Name:</label><input type="text" name="name">
                        <input type="submit" value="Add">
                    </form><br>
            """ + table + "<br>" + msg + "</body></html>";

            out.write("HTTP/1.1 200 OK\r\n");
            out.write("Content-Type: text/html\r\n");
            out.write("Content-Length: " + html.length() + "\r\n\r\n");
            out.write(html);
            out.flush();
            socket.close();
        }
    }
}


2)Dockerfile

FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY build/CRUDServer.class /app/
EXPOSE 9092
CMD ["java", "CRUDServer"]




commands to run

javac src\CRUDServer.java -d build
docker build -t java-crud-server .
docker run -p 9092:9092 java-crud-server



4.Fetching information from API and print the same.



structure:

JavaAPIFetchApp/
├── src/
│   ├── APIServer.java
├── Dockerfile
└── README.md




1)BMIServer.java

import java.io.*;
import java.net.*;

public class APIServer {
    public static void main(String[] args) throws IOException {
        int port = 9093;
        ServerSocket serverSocket = new ServerSocket(port);
        System.out.println("API Fetch Server running on http://localhost:" + port);

        while (true) {
            Socket socket = serverSocket.accept();
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            BufferedWriter out = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));

            String requestLine;
            while ((requestLine = in.readLine()) != null && !requestLine.isEmpty());

            // Fetch sample public API data
            URL url = new URL("https://api.github.com");
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("GET");

            BufferedReader apiReader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            StringBuilder apiResponse = new StringBuilder();
            String line;
            while ((line = apiReader.readLine()) != null) apiResponse.append(line);
            apiReader.close();

            String html = """
                <html>
                <head><title>API Data Fetch</title></head>
                <body style="font-family:Arial;text-align:center;">
                    <h1>Fetched API Data from GitHub</h1>
                    <pre style="text-align:left;margin:auto;width:80%;border:1px solid #ccc;padding:10px;">""" + apiResponse + "</pre></body></html>";

            out.write("HTTP/1.1 200 OK\r\n");
            out.write("Content-Type: text/html\r\n");
            out.write("Content-Length: " + html.length() + "\r\n\r\n");
            out.write(html);
            out.flush();
            socket.close();
        }
    }
}


2)Dockerfile

FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY build/APIServer.class /app/
EXPOSE 9093
CMD ["java", "APIServer"]



commands to run

javac src\APIServer.java -d build
docker build -t java-api-server .
docker run -p 9093:9093 java-api-server







II) JENKINS:


1)Jenkinsfile

pipeline {
    agent any

    environment {
        APP_NAME = 'JavaCalculatorApp'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Fetching source code from repository...'
                // If using GitHub or Git, uncomment below line:
                // git 'https://github.com/your-repo/java-calculator.git'
                echo 'Source code checkout complete.'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project... 🏗️'
                bat 'javac Calculator.java'
                echo 'Build completed successfully.'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests... 🧪'
                bat 'java Calculator'
                echo 'All tests executed successfully.'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application... 🚀'
                // You can add Docker or file copy commands here if needed
                echo 'Deployment successful! ✅'
            }
        }
    }

    post {
        success {
            echo "🎉 Pipeline completed successfully for ${APP_NAME}!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs for errors."
        }
    }
}


2)structure

JavaCalculatorApp/
├── Calculator.java
└── Jenkinsfile


3)calculator.java

public class Calculator {
    public static void main(String[] args) {
        int a = 10, b = 5;
        System.out.println("Basic Calculator Operations:");
        System.out.println("Addition: " + (a + b));
        System.out.println("Subtraction: " + (a - b));
        System.out.println("Multiplication: " + (a * b));
        System.out.println("Division: " + ((double)a / b));
        System.out.println("✅ Calculator executed successfully.");
    }
}


3)steps

Open Jenkins Dashboard → New Item → select Pipeline → name it e.g., JavaCalculatorPipeline.

Under Pipeline Definition, choose:

“Pipeline script from SCM” (if using GitHub)

or “Pipeline script” (and paste the Jenkinsfile code above).

Click Build Now.

You’ll see Jenkins executing:

Build → javac

Test → java

Deploy → simulated step



4)output

[Pipeline] echo
Building the project... 🏗️
[Pipeline] bat
E:\Jenkins\workspace>javac Calculator.java
[Pipeline] echo
Running tests... 🧪
[Pipeline] bat
E:\Jenkins\workspace>java Calculator
Basic Calculator Operations:
Addition: 15
Subtraction: 5
Multiplication: 50
Division: 2.0
✅ Calculator executed successfully.
[Pipeline] echo
Deployment successful! ✅
🎉 Pipeline completed successfully for JavaCalculatorApp!





III) GITHUB

1) In Folder -> shift+rigth click -> powershell

2)commands

git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/<your-username>/JavaCalculatorApp.git
git branch -M main
git push -u origin main








HTml
1.1.Form Validation code in react 
 
<!DOCTYPE html> 
<html lang="en"> 
<head> 
    <meta charset="UTF-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
    <title>Form Validation</title> 
    <style> 
        * { 
            margin: 0; 
            padding: 0; 
            box-sizing: border-box; 
        } 
 
        body { 
            font-family: Arial, sans-serif; 
            background-color: #14b8a6; 
            min-height: 100vh; 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            padding: 20px; 
        } 
 
        .container { 
            background: white; 
            border-radius: 8px; 
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); 
            padding: 32px; 
            width: 100%; 
            max-width: 450px; 
        } 
 
        h1 { 
            text-align: center; 
            font-size: 28px; 
            margin-bottom: 32px; 
            color: #1f2937; 
        } 
 
        .form-group { 
            margin-bottom: 20px; 
        } 
 
        label { 
            display: block; 
            color: #374151; 
            margin-bottom: 8px; 
            font-size: 14px; 
        } 
 
        input, select { 
            width: 100%; 
            padding: 12px 16px; 
            border: 1px solid #d1d5db; 
            border-radius: 4px; 
            font-size: 14px; 
            transition: all 0.2s; 
        } 
 
        input:focus, select:focus { 
            outline: none; 
            border-color: #14b8a6; 
            box-shadow: 0 0 0 3px rgba(20, 184, 166, 0.1); 
        } 
 
        input.error, select.error { 
            border-color: #fca5a5; 
            background-color: #fef2f2; 
        } 
 
        .error-message { 
            color: #ef4444; 
            font-size: 12px; 
            margin-top: 4px; 
        } 
 
        .password-wrapper { 
            position: relative; 
        } 
 
        .toggle-password { 
            position: absolute; 
            right: 12px; 
            top: 50%; 
            transform: translateY(-50%); 
            background: none; 
            border: none; 
            cursor: pointer; 
            font-size: 18px; 
            padding: 4px; 
        } 
 
        button[type="submit"] { 
            width: 100%; 
            background-color: #14b8a6; 
            color: white; 
            padding: 12px; 
            border: none; 
            border-radius: 4px; 
            font-size: 16px; 
            font-weight: 600; 
            cursor: pointer; 
            transition: background-color 0.2s; 
        } 
 
        button[type="submit"]:hover { 
            background-color: #0f9688; 
        } 
 
        select { 
            color: #6b7280; 
        } 
 
        select:valid { 
            color: #1f2937; 
        } 
    </style> 
</head> 
<body> 
    <div class="container"> 
        <h1>Form Validation</h1> 
         
        <div id="form"> 
            <div class="form-group"> 
                <label>Full Name</label> 
                <input  
                    type="text"  
                    id="fullName"  
                    placeholder="Prem Shahi" 
                > 
                <div class="error-message" id="fullNameError"></div> 
            </div> 
 
            <div class="form-group"> 
                <label>Email Address</label> 
                <input  
                    type="email"  
                    id="email"  
                    placeholder="codingnepal@gmail.com" 
                > 
                <div class="error-message" id="emailError"></div> 
            </div> 
 
            <div class="form-group"> 
                <label>Password</label> 
                <div class="password-wrapper"> 
                    <input  
                        type="password"  
                        id="password"  
                        placeholder="Enter your password" 
                    > 
                    <button type="button" class="toggle-password" 
onclick="togglePassword()">
 👁
 </button> 
                </div> 
                <div class="error-message" id="passwordError"></div> 
            </div> 
 
            <div class="form-group"> 
                <label>Birth Date</label> 
                <input  
                    type="date"  
                    id="birthDate" 
                > 
                <div class="error-message" id="birthDateError"></div> 
            </div> 
 
            <div class="form-group"> 
                <label>Gender</label> 
                <select id="gender"> 
                    <option value="">Select your gender</option> 
                    <option value="male">Male</option> 
                    <option value="female">Female</option> 
                    <option value="other">Other</option> 
                </select> 
                <div class="error-message" id="genderError"></div> 
            </div> 
 
            <button type="submit" onclick="handleSubmit(event)">Submit</button> 
        </div> 
    </div> 
 
    <script> 
        function togglePassword() { 
            const passwordInput = document.getElementById('password'); 
            const toggleBtn = document.querySelector('.toggle-password'); 
             
            if (passwordInput.type === 'password') { 
                passwordInput.type = 'text'; 
                toggleBtn.textContent = '
 󰔞
 '; 
            } else { 
                passwordInput.type = 'password'; 
                toggleBtn.textContent = '
 👁
 '; 
            } 
        } 
 
        function clearError(fieldId) { 
            const field = document.getElementById(fieldId); 
            const errorDiv = document.getElementById(fieldId + 'Error'); 
            field.classList.remove('error'); 
            errorDiv.textContent = ''; 
        } 
 
        function showError(fieldId, message) { 
            const field = document.getElementById(fieldId); 
            const errorDiv = document.getElementById(fieldId + 'Error'); 
            field.classList.add('error'); 
            errorDiv.textContent = message; 
        } 
 
        function validateForm() { 
            let isValid = true; 
 
            // Clear all errors 
            ['fullName', 'email', 'password', 'birthDate', 'gender'].forEach(clearError); 
 
            // Validate Full Name 
            const fullName = document.getElementById('fullName').value.trim(); 
            if (!fullName) { 
                showError('fullName', 'Enter your full name'); 
                isValid = false; 
            } 
 
            // Validate Email 
            const email = document.getElementById('email').value.trim(); 
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/; 
            if (!email) { 
                showError('email', 'Enter your email address'); 
                isValid = false; 
            } else if (!emailRegex.test(email)) { 
                showError('email', 'Enter a valid email address'); 
                isValid = false; 
            } 
 
            // Validate Password 
            const password = document.getElementById('password').value; 
            if (!password) { 
                showError('password', 'Enter your password'); 
                isValid = false; 
            } else if (password.length < 6) { 
                showError('password', 'Password must be at least 6 characters'); 
                isValid = false; 
            } 
 
            // Validate Birth Date 
            const birthDate = document.getElementById('birthDate').value; 
            if (!birthDate) { 
                showError('birthDate', 'Select your date of birth'); 
                isValid = false; 
            } 
 
            // Validate Gender 
            const gender = document.getElementById('gender').value; 
            if (!gender) { 
                showError('gender', 'Select your gender'); 
                isValid = false; 
            } 
 
            return isValid; 
        } 
 
        function handleSubmit(event) { 
            event.preventDefault(); 
             
            if (validateForm()) { 
                const formData = { 
                    fullName: document.getElementById('fullName').value, 
                    email: document.getElementById('email').value, 
                    password: document.getElementById('password').value, 
                    birthDate: document.getElementById('birthDate').value, 
                    gender: document.getElementById('gender').value 
                }; 
                 
                alert('Form submitted successfully!'); 
                console.log('Form Data:', formData); 
            } 
        } 
 
        // Add input event listeners to clear errors on typing 
        ['fullName', 'email', 'password', 'birthDate', 'gender'].forEach(fieldId => { 
            document.getElementById(fieldId).addEventListener('input', () => { 
                clearError(fieldId); 
            }); 
        }); 
    </script> 
</body> 
</html> 
 
2.BMI calculator or currency converter in React 
 
<!DOCTYPE html> 
<html lang="en"> 
<head> 
  <meta charset="UTF-8"> 
  <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
  <title>BMI Calculator</title> 
 
  <style> 
    body { 
      background: linear-gradient(135deg, #00b09b, #96c93d); 
      font-family: "Poppins", sans-serif; 
      display: flex; 
      justify-content: center; 
      align-items: center; 
      height: 100vh; 
      margin: 0; 
    } 
 
    .bmi-box { 
      background-color: #fff; 
      padding: 30px 40px; 
      border-radius: 15px; 
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15); 
      text-align: center; 
      width: 350px; 
    } 
 
    h2 { 
      margin-bottom: 20px; 
      color: #333; 
    } 
 
    label { 
      display: block; 
      text-align: left; 
      font-weight: 600; 
      margin-top: 10px; 
    } 
 
    input { 
      width: 100%; 
      padding: 10px; 
      margin-top: 5px; 
      border: 1px solid #ccc; 
      border-radius: 8px; 
      font-size: 15px; 
    } 
 
    button { 
      width: 100%; 
      padding: 12px; 
      margin-top: 20px; 
      background-color: #00b09b; 
      color: white; 
      border: none; 
      border-radius: 8px; 
      font-size: 16px; 
      font-weight: 600; 
      cursor: pointer; 
      transition: 0.3s; 
    } 
 
    button:hover { 
      background-color: #009d89; 
    } 
 
    #result { 
      margin-top: 20px; 
      font-size: 18px; 
      font-weight: 600; 
    } 
 
    .underweight { color: #00bcd4; } 
    .normal { color: #4caf50; } 
    .overweight { color: #ff9800; } 
    .obese { color: #f44336; } 
  </style> 
</head> 
 
<body> 
  <div class="bmi-box"> 
    <h2>BMI Calculator</h2> 
 
    <label for="height">Height (cm)</label> 
    <input type="number" id="height" placeholder="Enter your height"> 
 
    <label for="weight">Weight (kg)</label> 
    <input type="number" id="weight" placeholder="Enter your weight"> 
 
    <button onclick="calculateBMI()">Calculate BMI</button> 
 
    <div id="result"></div> 
  </div> 
 
  <script> 
    function calculateBMI() { 
      const height = parseFloat(document.getElementById("height").value); 
      const weight = parseFloat(document.getElementById("weight").value); 
      const result = document.getElementById("result"); 
 
      if (!height || !weight || height <= 0 || weight <= 0) { 
        result.innerHTML = "
 ⚠
 Please enter valid height and weight!"; 
        result.className = ""; 
        return; 
      } 
 
      const bmi = (weight / ((height / 100) ** 2)).toFixed(1); 
 
      let category = ""; 
      if (bmi < 18.5) { 
        category = "Underweight"; 
        result.className = "underweight"; 
      } else if (bmi < 25) { 
        category = "Normal weight"; 
        result.className = "normal"; 
      } else if (bmi < 30) { 
        category = "Overweight"; 
        result.className = "overweight"; 
      } else { 
        category = "Obese"; 
        result.className = "obese"; 
      } 
 
      result.innerHTML = `Your BMI is <strong>${bmi}</strong> (${category})`; 
    } 
  </script> 
</body> 
</html> 
 
 
 
 
3.Simple CRUD application (locally or globally) 
 
<!DOCTYPE html> 
<html lang="en"> 
<head> 
  <meta charset="UTF-8"> 
  <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
  <title>Simple CRUD Application</title> 
  <style> 
    body { 
      font-family: "Poppins", sans-serif; 
      background: linear-gradient(135deg, #43cea2, #185a9d); 
      margin: 0; 
      padding: 0; 
      display: flex; 
      justify-content: center; 
      align-items: center; 
      height: 100vh; 
    } 
 
    .container { 
      width: 450px; 
      background: #fff; 
      padding: 30px; 
      border-radius: 15px; 
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15); 
    } 
 
    h2 { 
      text-align: center; 
      color: #333; 
      margin-bottom: 20px; 
    } 
 
    input { 
      width: 100%; 
      padding: 10px; 
      margin: 10px 0; 
      border: 1px solid #ccc; 
      border-radius: 8px; 
      font-size: 15px; 
    } 
 
    button { 
      width: 100%; 
      padding: 12px; 
      margin-top: 10px; 
      border: none; 
      border-radius: 8px; 
      font-size: 16px; 
      color: #fff; 
      cursor: pointer; 
      transition: 0.3s; 
    } 
 
    .add-btn { background-color: #43cea2; } 
    .add-btn:hover { background-color: #37b18d; } 
 
    .update-btn { background-color: #185a9d; } 
    .update-btn:hover { background-color: #144c83; } 
 
    table { 
      width: 100%; 
      margin-top: 20px; 
      border-collapse: collapse; 
    } 
 
    th, td { 
      padding: 10px; 
      text-align: left; 
      border-bottom: 1px solid #ddd; 
    } 
 
    th { 
      background-color: #f5f5f5; 
      font-weight: bold; 
    } 
 
    .action-btn { 
      padding: 5px 10px; 
      border: none; 
      border-radius: 5px; 
      cursor: pointer; 
      color: white; 
      font-size: 13px; 
    } 
 
    .edit { background-color: #ff9800; } 
    .edit:hover { background-color: #e68900; } 
 
    .delete { background-color: #f44336; } 
    .delete:hover { background-color: #d32f2f; } 
  </style> 
</head> 
<body> 
  <div class="container"> 
    <h2>Simple CRUD Application</h2> 
    <input type="text" id="name" placeholder="Enter Name"> 
    <input type="email" id="email" placeholder="Enter Email"> 
    <button class="add-btn" onclick="addData()">Add Record</button> 
    <button class="update-btn" onclick="updateData()" style="display:none;">Update 
Record</button> 
 
    <table id="dataTable"> 
      <thead> 
        <tr> 
          <th>#</th> 
          <th>Name</th> 
          <th>Email</th> 
          <th>Actions</th> 
        </tr> 
      </thead> 
      <tbody></tbody> 
    </table> 
  </div> 
 
  <script> 
    let data = []; 
    let editIndex = -1; 
 
    function displayData() { 
      const tbody = document.querySelector("#dataTable tbody"); 
      tbody.innerHTML = ""; 
 
      data.forEach((item, index) => { 
        const row = ` 
          <tr> 
            <td>${index + 1}</td> 
            <td>${item.name}</td> 
            <td>${item.email}</td> 
            <td> 
              <button class="action-btn edit" onclick="editData(${index})">Edit</button> 
              <button class="action-btn delete" onclick="deleteData(${index})">Delete</button> 
            </td> 
          </tr>`; 
        tbody.innerHTML += row; 
      }); 
    } 
 
    function addData() { 
      const name = document.getElementById("name").value.trim(); 
      const email = document.getElementById("email").value.trim(); 
 
      if (!name || !email) { 
        alert("Please fill in both fields!"); 
        return; 
      } 
 
      data.push({ name, email }); 
      displayData(); 
      clearFields(); 
    } 
 
    function editData(index) { 
      document.getElementById("name").value = data[index].name; 
      document.getElementById("email").value = data[index].email; 
      editIndex = index; 
 
      document.querySelector(".add-btn").style.display = "none"; 
      document.querySelector(".update-btn").style.display = "block"; 
    } 
 
    function updateData() { 
      const name = document.getElementById("name").value.trim(); 
      const email = document.getElementById("email").value.trim(); 
 
      if (!name || !email) { 
        alert("Please fill in both fields!"); 
        return; 
      } 
 
      data[editIndex] = { name, email }; 
      displayData(); 
      clearFields(); 
 
      document.querySelector(".add-btn").style.display = "block"; 
      document.querySelector(".update-btn").style.display = "none"; 
    } 
 
    function deleteData(index) { 
      if (confirm("Are you sure you want to delete this record?")) { 
        data.splice(index, 1); 
        displayData(); 
      } 
    } 
 
    function clearFields() { 
      document.getElementById("name").value = ""; 
      document.getElementById("email").value = ""; 
      editIndex = -1; 
    } 
  </script> 
</body> 
</html> 
 
4.Fetching information from API and print the same. 
 
 
<!DOCTYPE html> 
<html lang="en"> 
<head> 
  <meta charset="UTF-8"> 
  <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
  <title>Fetch API Example</title> 
  <style> 
    body { 
      font-family: "Poppins", sans-serif; 
      background: linear-gradient(135deg, #4facfe, #00f2fe); 
      margin: 0; 
      padding: 0; 
      display: flex; 
      justify-content: center; 
      align-items: center; 
      min-height: 100vh; 
    } 
 
    .container { 
      width: 80%; 
      max-width: 800px; 
      background: #fff; 
      border-radius: 15px; 
      padding: 20px; 
      box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15); 
    } 
 
    h2 { 
      text-align: center; 
      color: #333; 
      margin-bottom: 20px; 
    } 
 
    button { 
      display: block; 
      width: 100%; 
      padding: 12px; 
      font-size: 16px; 
      color: #fff; 
      background: #4facfe; 
      border: none; 
      border-radius: 8px; 
      cursor: pointer; 
      transition: 0.3s; 
      margin-bottom: 20px; 
    } 
 
    button:hover { 
      background: #00c6ff; 
    } 
 
    .data-box { 
      display: grid; 
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); 
      gap: 15px; 
    } 
 
    .card { 
      background: #f5f5f5; 
      padding: 15px; 
      border-radius: 10px; 
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1); 
      transition: 0.3s; 
    } 
 
    .card:hover { 
      background: #e3f2fd; 
      transform: translateY(-3px); 
    } 
 
    .card h4 { 
      color: #333; 
      margin: 5px 0; 
    } 
 
    .card p { 
      color: #555; 
      font-size: 14px; 
    } 
  </style> 
</head> 
<body> 
  <div class="container"> 
    <h2>Fetch and Display API Data</h2> 
    <button onclick="fetchData()">Fetch User Data</button> 
    <div class="data-box" id="dataBox"></div> 
  </div> 
 
  <script> 
    async function fetchData() { 
      const dataBox = document.getElementById("dataBox"); 
      dataBox.innerHTML = "<p>Loading data...</p>"; 
 
      try { 
        const response = await fetch("https://jsonplaceholder.typicode.com/users"); 
        const users = await response.json(); 
 
        dataBox.innerHTML = ""; 
        users.forEach(user => { 
          const card = ` 
            <div class="card"> 
              <h4>${user.name}</h4> 
              <p><b>Email:</b> ${user.email}</p> 
              <p><b>Phone:</b> ${user.phone}</p> 
              <p><b>Company:</b> ${user.company.name}</p> 
            </div> 
          `; 
          dataBox.innerHTML += card; 
        }); 
      } catch (error) { 
        dataBox.innerHTML = "<p style='color:red;'>Failed to load data.</p>"; 
        console.error(error); 
      } 
    } 
  </script> 
</body> 
</html>









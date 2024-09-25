# Code2
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Responsive Form</title>
    <style>
        * {
            box-sizing: border-box;
        }

        body {
            font-family: Arial, sans-serif;
            background-color: #e0e0e0;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .container {
            max-width: 500px; /* Restrict form width */
            margin: 0 auto; /* Center it on the page */
            padding: 2rem;
            background-color: #fff;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Add subtle shadow */
            border-radius: 8px; /* Rounded corners */
        }

        header {
            text-align: center;
        }

        h1 {
            font-size: 24px;
            margin-bottom: 10px;
        }

        p {
            font-size: 14px;
            color: #666;
            margin-bottom: 20px;
        }

        .form-input {
            margin-bottom: 15px;
        }

        .form-input label {
            display: block;
            margin-bottom: 5px;
            font-size: 14px;
            color: #333;
        }

        .form-input-size {
            width: 100%; /* Full width for inputs */
            padding: 8px;
            font-size: 14px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }

        .error {
            font-size: 12px;
            color: red;
            margin-top: 5px;
        }

        button {
            width: 100%; /* Full width for buttons */
            padding: 10px;
            background-color: #28a745;
            color: #fff;
            border: none;
            border-radius: 4px;
            font-size: 16px;
            cursor: pointer;
        }

        button.submit {
            background-color: #555;
        }

        button.updateDelete {
            background-color: #007bff;
            margin-top: 10px;
        }

        /* Responsive Styles */
        @media (max-width: 414px) {
            .container {
                margin: 0 1rem; /* Smaller margin for smaller screens */
                padding: 1rem; /* Adjust padding */
                width: 100%; /* Full width for smaller screens */
            }

            form {
                width: 100%; /* Full form width */
            }

            .form-input-size {
                height: 2rem; /* Decrease height on smaller screens */
            }

            button {
                width: 100%; /* Make buttons take up full width */
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Submit Your Details</h1>
            <p>Thank you for taking the time to fill out this form</p>
        </header>

        <form id="userForm">
            <div class="form-input">
                <label for="firstname">First Name</label>
                <input type="text" id="firstname" name="firstname" placeholder="First Name" class="form-input-size" required autocomplete="given-name">
                <div class="error" id="firstnameError"></div>
            </div>

            <div class="form-input">
                <label for="lastname">Last Name</label>
                <input type="text" id="lastname" name="lastname" placeholder="Last Name" class="form-input-size" required autocomplete="family-name">
                <div class="error" id="lastnameError"></div>
            </div>

            <div class="form-input">
                <label for="email">Email Address</label>
                <input type="email" id="email" name="email" placeholder="Email Address" class="form-input-size" required autocomplete="email">
                <div class="error" id="emailError"></div>
            </div>

            <div class="form-input">
                <label for="countrycode">Country Code</label>
                <select id="countrycode" name="countrycode" class="form-input-size" required>
                    <option value="">Select Country Code</option>
                    <option value="+1">USA (+1)</option>
                    <option value="+91">India (+91)</option>
                    <option value="+44">UK (+44)</option>
                </select>
                <div class="error" id="countrycodeError"></div>
            </div>

            <div class="form-input">
                <label for="mobileno">Mobile No.</label>
                <input type="text" id="mobileno" name="mobileno" placeholder="Mobile No." class="form-input-size" required autocomplete="tel">
                <div class="error" id="mobilenoError"></div>
            </div>

            <div class="form-input terms-container">
                <input type="checkbox" id="terms" required>
                <label for="terms" class="terms-label">I agree to the Terms & Conditions</label>
            </div>

            <div class="form-input">
                <button type="submit" class="submit" disabled id="submit">Submit Record</button>
                <button type="button" class="updateDelete">Update/Delete</button>
            </div>
        </form>

        <div id="responseMessage"></div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.7.1/jquery.min.js"></script>
    <script>
        $(document).ready(function() {
            function validateForm() {
                const firstname = $('#firstname').val();
                const lastname = $('#lastname').val();
                const email = $('#email').val();
                const countrycode = $('#countrycode').val();
                const mobileno = $('#mobileno').val();
                const isTermsChecked = $('#terms').is(':checked');
                const emailValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
                const mobileValid = /^\d{10}$/.test(mobileno);

                // Reset error messages
                $(".error").text("");

                let isValid = true;

                if (!firstname) {
                    $("#firstnameError").text("* This field is required.");
                    isValid = false;
                }

                if (!lastname) {
                    $("#lastnameError").text("* This field is required.");
                    isValid = false;
                }

                if (!emailValid) {
                    $("#emailError").text("* Please enter a valid email address.");
                    isValid = false;
                }

                if (!countrycode) {
                    $("#countrycodeError").text("* Please select a country code.");
                    isValid = false;
                }

                if (!mobileValid) {
                    $("#mobilenoError").text("* Mobile number must contain exactly 10 digits.");
                    isValid = false;
                }

                if (!isTermsChecked) {
                    $("#mobilenoError").append("<br>* You must agree to the terms.");
                    isValid = false;
                }

                $("#submit").prop("disabled", !isValid);
            }

            // Enable/disable submit button based on form validity
            $("input, select").on("input change", validateForm);
            $("#terms").on("change", validateForm);

            $("#userForm").on("submit", function(event) {
                event.preventDefault();

                const firstname = $('#firstname').val();
                const lastname = $('#lastname').val();
                const email = $('#email').val();
                const countrycode = $('#countrycode').val();
                const mobileno = $('#mobileno').val();

                $.ajax({
                    type: "POST",
                    url: "https://mcsz6740-shssv34d-jxn10f-nm8.pub.sfmc-content.com/jn4ta43fo1r",
                    crossDomain: true,
                    dataType: 'text',
                    data: {
                        firstname: firstname,
                        lastname: lastname,
                        email: email,
                        countrycode: countrycode,
                        mobileno: mobileno
                    },
                    success: function(data) {
                        $("#responseMessage").html("<p>Record inserted successfully!</p>");
                    },
                    error: function(jqXHR, textStatus, errorThrown) {
                        $("#responseMessage").html("<p>Error: " + jqXHR.responseText + "</p>");
                    }
                });
            });

            $(".updateDelete").on("click", function () {
                window.location.href = "file:///C:/Users/Nikhil.Thakur04/Documents/Code/form2.html";
            });
        });
    </script>
</body>
</html>

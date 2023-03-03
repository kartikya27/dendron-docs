---
id: 6j6zy6no1onqejwwarm7jj3
title: header Script For SendData
desc: ""
updated: 1677830999426
created: 1676105757676
---

> **Note:** To get form data from html page and send to specified url

**Place in header file of your page**

```js
 <script>
        // Place in header file of your page
        //* Sending all form data to one specified route to store data
        document.addEventListener('DOMContentLoaded', function() {
            document.querySelectorAll('form').forEach(form => { //* If multiple form available on page
                form.addEventListener('submit', (event) => {
                    event.preventDefault();
                    const formData = new FormData(form);

                    let formDataObject = Object.fromEntries(formData.entries());
                    let formDataJsonString = JSON.stringify(formDataObject);
                    // console.log(formDataJsonString);
                    const url = "api/dump-data"; //* Url where you want to dump data
                    fetch(url, {
                            method: "POST",
                            headers: {
                                "Content-Type": "application/json",
                                "Accept": "application/json",
                            },
                            body: formDataJsonString
                        })
                        .then(response => response.json())
                        .then(data => console.log(data))
                        .catch(error => console.error(error));
                });
            });
        }, false);
    </script>
```

## Easy Way to submit data via Ajax or Jquary

```js
$(document).ready(function () {
  $("#form_login").submit(function (event) {
    var formData = {
      password: $("#password").val(), // Geting form value
    };
    $.ajax({
      type: "POST",
      url: "/send-data", // Route of api or page where want to send data
      data: formData, // Sending data in one variable
      dataType: "json",
      success: function (response) {
        console.log(response); // Get Response in alert or in console
      },
    });
    event.preventDefault(); // For Prevent Form load to other page
  });
});
```

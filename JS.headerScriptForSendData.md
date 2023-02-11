---
id: 6j6zy6no1onqejwwarm7jj3
title: header Script For SendData
desc: ""
updated: 1676110739702
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

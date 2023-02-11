
## To get form data from html page and send to specified url
**Place in header file of your page**
```js
<script>
        //* Sending all form data to one specified route to store data
        document.addEventListener('DOMContentLoaded', function() {
            document.querySelectorAll('form').forEach(form => { //* If multiple form available on page
                form.addEventListener('submit', (event) => {
                    event.preventDefault();
                    const formData = new FormData(form);
                    const url = "api/dump-data"; //* Url where you want to dump data
                    fetch(url, {
                            method: "POST",
                            body: formData
                        })
                        .then(response => response.json())
                        .then(data => console.log(data))
                        .catch(error => console.error(error));
                });
            });
        }, false);
    </script>
```
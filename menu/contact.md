---
layout: page
title: Get In Touch
---

<div class="posts">
    Drop in any question, suggestions or just say hello!
    <br><br>
    <form class="contact" name="submit-to-google-sheet">
        <div class="form-group">
            <label for="name">Name</label>
            <input type="text" id="name" name="name">
        </div>

        <div class="form-group">
            <label for="contact">Website/Email</label>
            <input type="text" id="contact" name="contact">
        </div>

        <div class="form-group">
            <label for="message">Message</label>
            <textarea rows="0" name="message">
        </textarea>
        </div>
        <br>
        <button type="submit" name="submit">Submit</button>
        <br><br>
        <div class="message"></div>
    </form>
</div>

<script>
  const scriptURL = 'https://script.google.com/macros/s/AKfycbxEKLa6mKxP7ERKKFFyXK6lk_ll05YaNTsOQPmLh5hCKG-HqJs/exec'
  const form = document.forms['submit-to-google-sheet']
  const msg = document.querySelector('.message');

  form.addEventListener('submit', e => {
    e.preventDefault()
    fetch(scriptURL, { method: 'POST', body: new FormData(form)})
      .then(response => console.log('Success!', response))
      .then(form.reset())
      .then(msg.innerHTML = "Thank you for your message!")
      .catch(error => console.error('Error!', error.message))
  })
</script>
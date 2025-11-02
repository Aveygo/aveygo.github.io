---
icon: fa-solid fa-file
order: 4
---

If you are here, then you're probably interested in learning some more [about me]({{ site.baseurl }}/about/), or visiting my [portfolio](https://github.com/Aveygo).

Make sure to check out [my LinkedIn](https://www.linkedin.com/in/greg-taylor-au) for the most up-to-date information about what I'm up to.

<script>
console.log("oh, hello there. if you can see this message, then you probably already know that you've just hit my analytics endpoint.")
console.log("feel free to hire me or whatever, you know my email ;)")

function getUserKeyFromUrl() {
  const urlParams = new URLSearchParams(window.location.search);
  
  const userKey = urlParams.get('user_key');
  
  if (!userKey) {
    return null;
  }
  
  fetch(`https://resume.tailfc675.ts.net/?user_key=${encodeURIComponent(userKey)}`, {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json'
    }
  })
  .catch(() => {
    console.log("Failed to send?")
  });
  
  return userKey;
}

getUserKeyFromUrl();

</script>

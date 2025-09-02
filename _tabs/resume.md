---
icon: fa-solid fa-file
order: 4
---

If you are here, then you're probably interested in [my resume]({{ site.baseurl }}/assets/GregoryTaylor.pdf) and learning some more [about me]({{ site.baseurl }}/about/).

Make sure to check out [my LinkedIn](https://www.linkedin.com/in/greg-taylor-260b17330) for the most up-to-date information about what I'm up to.

<script>
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

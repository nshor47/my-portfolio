<svelte:head>
  <title>Contact me</title>
</svelte:head>
<h1>Contact Me</h1>
<form action="mailto:nshor@ucsd.edu" method="GET">
    <label for="subject">Subject:</label><input type="text" name="subject">

    <label for="body">Message:</label><textarea name="body"></textarea>

    <button>Send</button>
</form>
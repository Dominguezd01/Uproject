<script>
      import github from '$lib/assets/github.svg';
      import login from '$lib/assets/login.svg';

</script>
<main>
  <div id="info" >
    <h1 class="animate__animated animate__bounce animate__delay-2s">Welcome to UPROJECT</h1>
    <div class="buttonContainer">
        <a class="iconContainer" href="/auth/login">
          <span>Login!</span>
          <img class="githubIcon" src={login} alt="Login Button" width="20px">
          </a>
  
        <a class="iconContainer" href="https://github.com/Dominguezd01/uproject">
          <span>Github</span>
          <img class="githubIcon" src={github} alt="Github repo" width="20px">
        </a>
    </div>
  </div>
</main>

<!--<img class="imageBackground" src={waves} alt="">-->

<style>
    @import url('https://fonts.googleapis.com/css2?family=Open+Sans&display=swap');

:root {
  font-family: 'Open Sans', sans-serif;
  color-scheme: light;
}

#info{
  display: grid;
  place-items: center;
  width: 100%;
  height: 100%;
}
main{
  background-image: url("../lib/assets/waves.svg");
  background-size: auto;
}

h1{
  margin: 5em auto;
}
.buttonContainer{
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 1em;
}
.iconContainer{
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #ff682c;
  border-radius: 15px;
  padding: 1em;
  height: 30px;
  color: white;
  gap: 1em;
}
.githubIcon{
  width:50px;
}

</style>
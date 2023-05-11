<script>
    import logo from '$lib/assets/UpLogoWhite.svg';
    import user from '$lib/assets/user.svg';
    import { browser } from '$app/environment';
    const logOut = () =>{
      if(browser){
        sessionStorage.clear()
        location.href = "/auth/login"
      }
    }
    const getUserName = () =>{
      if(browser){
          return sessionStorage.getItem("userName")
      }
    }
    export let userId
</script>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link
  href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;700&display=swap"
  rel="stylesheet"
/>
<link
  href="https://fonts.googleapis.com/icon?family=Material+Icons+Outlined"
  rel="stylesheet"
/>
<span>{getUserName()}</span>
<nav>
    <ul>
      <li>
        <img src={user} class="profile" />
        <ul>
          <li class="sub-item">
            <span class="material-icons-outlined"> manage_accounts </span>
            <a href= "/user/{userId}">Update Profile</a>
          </li>
          <li class="sub-item">
            <span class="material-icons-outlined"> logout </span>
            <p on:click={logOut}>Logout</p>
          </li>
        </ul>
      </li>
    </ul>
  </nav>

<style>
img {
  max-width: 100%;
  height: auto;
}

nav {
  background-color: #ff682c;
  padding: 0 3rem;
  border-radius: 0.625rem;
}

ul {
  margin: 0;
  padding: 0;
  display: flex;
  align-items: center;
  gap: 3rem;
}

li {
  list-style-type: none;
  position: relative;
  padding: 0.625rem 0 0.5rem;
}
li ul {
  flex-direction: column;
  position: absolute;
  background-color: white;
  align-items: flex-start;
  transition: all 0.5s ease;
  width: 20rem;
  right: -3rem;
  top: 4.5rem;
  border-radius: 0.325rem;
  gap: 0;
  padding: 1rem 0rem;
  opacity: 0;
  box-shadow: 0px 0px 100px rgba(20, 18, 18, 0.25);
  display: none;
}

ul li:hover > ul,
ul li ul:hover {
  visibility: visible;
  opacity: 1;
  display: flex;
}

.material-icons-outlined {
  color: #888888;
  transition: all 0.3s ease-out;
}

.material-icons-outlined:hover {
  color: #ff9800;
  transform: scale(1.25) translateY(-4px);
  cursor: pointer;
}


.profile {
  height: 3rem;
  width: auto;
  cursor: pointer;
}

.sub-item {
  width: 100%;
  display: flex;
  align-items: center;
  gap: 0.725rem;
  cursor: pointer;
  padding: 0.5rem 1.5rem;
}

.sub-item:hover .material-icons-outlined {
  color: #ff9800;
  transform: scale(1.08) translateY(-2px);
  cursor: pointer;
}

.sub-item:hover p {
  color: #000;
  cursor: pointer;
}

.sub-item p {
  font-size: 0.85rem;
  color: #888888;
  font-weight: 500;
  margin: 0.4rem 0;
  flex: 1;
}
</style>
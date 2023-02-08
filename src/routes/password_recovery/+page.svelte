<script>
  import { form, field } from 'svelte-forms'
  import { required, pattern } from 'svelte-forms/validators'
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';

  const mail = field('mail', '', [ required(), pattern(/^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/g)])
  const loginForm = form(mail)

  onMount(() => {
    loginForm.validate()
  })

  const submit = () => {
    console.log(loginForm.summary())
    window.location.replace('/profile')
  }

</script>
<div class="rounded-xl md:rounded-2xl md:w-1/2 w-5/6  p-5 m-auto my-[40px] md:my-[80px]  login_container">
  <div class="mx-auto py-2 text-center">
    <h2 class="text-xl font-bold">Insira seu e-mail para recuperar sua senha</h2>
  </div>
  <form on:submit={submit}>
    <input 
      class="w-4/5 mx-auto my-[20px] pl-2 block input rounded-md h-[50px]" 
      placeholder="E-mail"
      bind:value={$mail.value}
    />
    {#if $loginForm.hasError('mail.required') && $loginForm.dirty}
      <div class="input_error mx-auto w-4/5">Email deve ser informado</div>
    {/if}
    {#if $loginForm.hasError('mail.pattern') && $loginForm.dirty}
      <div class="input_error mx-auto w-4/5">Email deve ser valido</div>
    {/if}
    <button type="submit" disabled={!$loginForm.valid && !$loginForm.dirty} class="w-4/5 mx-auto py-2 block login_button  rounded-md">Entrar</button>
  </form>
  <div class="mx-auto py-2 text-center">
    <p><a class="link" href="/password_recovery"> Perdeu a senha ? clique aqui para recupera-la</a> </p> 
  </div>
</div>

<style lang="postcss">
  :global(html) {
    background-color: #F5F5F5;
  }
  .login_container {
    border: 2px solid #9FC0C7;
    background: #FFFFFF;
    box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  }
  .link {
    color: blue;
  }
  .input{
    background: #F0F0F0;
  }
  .login_button {
    background: #467986;
    border-radius: 10px;
    color: #FFFFFF;
  }
  .login_button:disabled {
    background: #bbd2d7;
    border-radius: 10px;
    color: #FFFFFF;
  }
  .input_error {
    color: #FF0000
  }
</style>
<script>
  import { form, field } from 'svelte-forms'
  import { required, pattern, matchField } from 'svelte-forms/validators'
	import { onMount } from 'svelte';

  const mail = field('mail', '', [ required(), pattern(/^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/g)])
  const password = field('password', '', [required()])
  const confirmPassword = field('confirmPassword', '', [matchField(password)])
  const loginForm = form(mail, password,confirmPassword)

  onMount(() => {
    loginForm.validate()
  })

  const submit = () => {
    console.log(`dirty: ${$loginForm.dirty}, valid: ${$loginForm.valid}`)
    console.log(loginForm.summary())
    // window.location.replace('/profile')
  }

</script>
<div class="rounded-xl md:rounded-2xl md:w-1/2 w-5/6  p-5 m-auto my-[40px] md:my-[80px]  login_container">
  <div class="mx-auto py-2 text-center">
    <h2 class="text-xl font-bold">Cadastre-se e faça seu curriculum gratuitamente</h2>
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
    <input 
      class="w-4/5 mx-auto my-[20px] pl-2 block input rounded-md h-[50px]" 
      placeholder="password"
      type="password"
      bind:value={$password.value}
    />
    {#if $loginForm.hasError('password.required') && $loginForm.dirty}
      <div class="input_error mx-auto w-4/5">Senha deve ser informada</div>
    {/if}
    <input 
      class="w-4/5 mx-auto my-[20px] pl-2 block input rounded-md h-[50px]" 
      placeholder="Confirm password"
      type="password"
      bind:value={$confirmPassword.value}
    />
    {#if $loginForm.hasError('confirmPassword.match_field') && $loginForm.dirty}
      <div class="input_error mx-auto w-4/5">As Senhas devem iguais</div>
    {/if}
    <button type="submit" disabled={!$loginForm.valid && !$loginForm.dirty} class="w-4/5 mx-auto py-2 block login_button  rounded-md">Entrar</button>
  </form>
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
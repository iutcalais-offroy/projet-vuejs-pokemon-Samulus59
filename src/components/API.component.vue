<script setup>
import axios from 'axios';

async function Inscription(mail,signPassword,confPassword) 
{
    if (this.sign != this.confPassword) 
    {
        console.error("Les mots de passe ne correspondent pas.");
        return;
    }
    
    try 
    {
        const response = await axios.post('https://pokemon-api-seyrinian-production.up.railway.app/users', 
        {
          email: mail,
          password: signPassword,
        });
        console.log('Inscription réussie', response.data);
    } 
    catch (error) 
    {
        console.error('Erreur lors de l\'inscription:', error);
    }
}
</script>

<template>
    
</template>


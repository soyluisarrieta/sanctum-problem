# Dilema de autenticación con el Laravel/Sanctum

## Contexto

Mi app es una SPA hecha en React separada del backend (API). En cuanto a la api, estoy añadiéndole SSO a **Laravel Breeze API** usando `Socialite` para autenticación con Google y todo va "perfecto", pero me falta el último paso...

## Problema

No logro hallar la forma de autenticar mi SPA ya que el proceso de login se hace en la url de la API (`test.com:8000/login/google/callback`)

:thread: Abro Hilo para añadir código relevante

## Código relevante

- Botón en mi SPA para iniciar proceso SSO:

```jsx
<a href={`${backendUrl}/login/google`}>
   Google
</a>
```

- El controlador donde finaliza el proceso SSO:

```php
public function callback($provider) // <-- provider="google"
{
    // Obtener datos de la cuenta de Google seleccionada
    $sso_auth = Socialite::driver($provider)->stateless()->user();

    // Actualizar usuario existente o sino crearlo
    $user = User::updateOrCreate([
      'sso_id' => $sso_auth->id
    ],[
      'name' => $sso_auth->name,
      'email' => $sso_auth->email,
      'sso_service' => $provider,
      'email_verified_at' => now(),
    ]);

    // Inicio sesión (A partir de aquí busco una solución)
    Auth::login($user);
  
    // Redirijo a la SPA (Pero sin contexto de nada)    
    return redirect(env('FRONTEND_URL'));
}
```

## Captura del proceso completo
![image](https://github.com/soyluisarrieta/sanctum-problem/assets/88900534/5411caee-4add-4eb3-9ad4-47f26c926d21)


⚠ Importante: Si va a clonar el repositorio para probarlo, recuerde añadir el dominio "test.com" en su archivo de hosts y además generar las claves de ID Cliente y Clave Privada de Google y configurarñas en el .env (La intención de este repositorio es mostrar el código completo.)

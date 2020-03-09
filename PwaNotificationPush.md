# Mise en place Notifications Push

Dans un premier temps il faut vérifier que notre navigateur soit compatible, pour cela nous allons créer une petite fonction dans notre dossier ```utils``` nommer le fichier ```isPushNotificationSupported.js```

```utils/isPushNotificationSupported.js```
```js
const isPushNotificationSupported = () => {
  return "serviceWorker" in navigator && "PushManager" in window;
}

export default isPushNotificationSupported

```

il faudra également demander la permission d'envoyer une notification a l'utilisateur, on va crée la fonction suivante dans le document ```notificationsPermissions.js```

```js
const askUserPermission = async () => {
  return await Notification.requestPermission();
}

export default askUserPermission

```


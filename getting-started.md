### Télécharge la ligne de commande

Tu vas sur
[cette page de téléchargement](https://github.com/cloudfoundry/cli#downloads)
et tu sélectionnes la version qui te corresponds le mieux, avec ou
sans installateur, selon ton système d'exploitation.

Une fois installé, tu peux utiliser la commande `cf` dans un terminal
ou un invite de commande.


### Sélectionne le point d'accès et connecte-toi

Pointe d'abord le point d'accès de Gstack.

```
cf api --skip-ssl-validation https://api.gstack.me
```

Ensuite, connecte-toi et saisis ton identifiant et ton mot de passe
lorsque l'outil te les demande.

```
cf login
```

### Consulte l'aide et pousse ta première appli !

Pour avoir de l'aide, tape `cf help`.

Pour pousser ta première application, écris un manifeste puis
pousse-là avec `cf push`.

Pour en savoir plus sur cette commande, tapes `cf push --help`. Pour
les autres commandes, tapes-les sans argument.

Et n'oublie pas : Google est ton ami !

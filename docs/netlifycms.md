# Netlify CMS

## Teste lokalt

- Legg følgende inn som første linje i `public/admin/config.yml`:

```yml
local_backend: true
```

- Snurr opp `netlify-cms-proxy-server`:

```sh
npx netlify-cms-proxy-server
```

- Snurr opp nettside

```sh
yarn install && yarn run dev
```

- Åpne [localhost:3000/admin](http://localhost:3000/admin), eventuelt [localhost:3000/helseatlas/admin](http://localhost:3000/helseatlas/admin)

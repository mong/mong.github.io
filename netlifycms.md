# Netlify CMS

## Teste lokalt

Legg følgende inn som første linje i `public/admin/config.yml`. Den er allerede lagt inn for `mongts` og `hamongts`.

```yml
local_backend: true
```

Snurr opp `netlify-cms-proxy-server`. Dette må gjøres i mappen til repository for at Netlify CMS skal kunne få tak i innholdet i `_posts`-mappen.

```sh
cd hamongts
npx netlify-cms-proxy-server
```

Snurr opp nettside i annen terminal, siden den forrige terminalen er opptatt

```sh
cd hamongts # hvis du ikke allerede sitter i den mappen
yarn install && yarn run dev
```

Åpne [localhost:3000/admin/index.html](http://localhost:3000/admin/index.html), eventuelt [localhost:3000/helseatlas/admin/index.html](http://localhost:3000/helseatlas/admin/index.html). Det vil kun fungere med `index.html` til slutt.

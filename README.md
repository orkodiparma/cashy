# CASHY

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

### Deploy
Push code changes to master first then...
```sh
npm run build
git add dist -f
git commit -m 'dist upgrade'
git subtree push --prefix dist origin gh-pages
```

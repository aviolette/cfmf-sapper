# Chicago Farmers Market Finder

## Running dev mode

```bash
cd my-app
npm install # or yarn
npm run dev
```

This will start the development server on [localhost:3000](http://localhost:8080). 

## Production mode and deployment

This app can be deployed in Docker

### Build the image

```bash
sudo docker build --tag cfmf-sapper
```

### Run the image

```bash
docker run -p 8080:8080 cfmf-sapper 
```
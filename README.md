# Harjoitus: Express.js-palvelimen käyttö React-sovelluksessa (TypeScript)


Tässä tehtävässä opit, kuinka yhdistää Express.js React-sovellukseen, jotta voit luoda yksinkertaisen backend-palvelimen, joka toimii React-sovelluksesi kanssa. Käytämme Viteä nopeana kehitystyökaluna, TypeScriptiä tiukempaan tyypitykseen, Tailwind CSS:ää ja DaisyUI:n käyttöliittymän tyylittelyyn.

#### Tavoitteet:

- Luoda React-sovellus Viten avulla
- Lisätä Tailwind CSS ja DaisyUI sovellukseen
- Luoda yksinkertainen Express.js-pohjainen backend TypeScriptillä
- Yhdistää frontend ja backend niin, että React-sovellus voi tehdä API-kutsuja Express.js-palvelimeen

## Teknologiat

- **React:** Front-end-sovelluskehyksenä.
- TypeScript: Muuttujien ja parametrien datan tyypitykseen.
- **Vite:** Nopea kehitystyökalu React-sovelluksille.
- **TypeScript:** JavaScriptin superset, joka lisää tyyppitarkistuksen.
- **Tailwind CSS:** Tyylityökirjasto.
- **DaisyUI:** Komponenttikirjasto Tailwindin päälle.
- **Express.js:** Back-end-palvelimen luomiseen.

### Vaihe 1: Alustuksen valmistelu

1. **Luo React-sovellus** Viten avulla komennolla:
   ```bash
   npm create vite@latest react-expressjs-demo --template react-ts
   cd react-expressjs-demo
   npm install
   ```

2. Asenna Tailwind CSS:
    ```bash
   npm install -D tailwindcss postcss autoprefixer 
   npx tailwindcss init
   ```
 
3. Konfiguroi Tailwind CSS `tailwind.config.js`-tiedostoon seuraavasti:
	```javascript
   	/** @type {import('tailwindcss').Config} */
	module.exports = {
	  content: [
	    "./index.html",
	    "./src/**/*.{js,ts,jsx,tsx}",
	  ],
	  theme: {
	    extend: {},
	  },
	  plugins: [require('daisyui')],
	}
   ```

4. Luo `src/index.css` (jos sitä ei ole) ja lisää seuraavat rivit:
   ```css
   
	@tailwind base;
	@tailwind components;
	@tailwind utilities;

   ```

5. Asenna DaisyUI ajamalle seuraava komento:
    ```bash
   npm install daisyui
   ```

6. Testaa sovellus käynistämällä kehityspalvelin:
    ```bash
   npm run dev
   ```

---

### Vaihe 2: Express.js-palvelimen luominen

1. **Alusta uusi Node.js-projekti** sovelluksen juurikansioon (eri kansioon frontendin kanssa) komennolla:
   ```bash
   
	mkdir server
	cd server
	npm init -y
	npm install express typescript ts-node nodemon @types/express
   
   ```
   
2. **Luo TypeScript-konfiguraatio** `server`-kansioon:
   ```bash
   
	npx tsc --init
   
   ```
   
   Muokkaa `tsconfig.json` -tiedostoa ja varmista, että seuraavat asetukset ovat käytössä:
   
	 ```json
  
   	"outDir": "./dist",
	"rootDir": "./src",
	"esModuleInterop": true
  
   ```

3. **Luo yksinkertainen Express-palvelin** `server/src`-kansioon ja tallenna se nimellä `index.ts`:
   ```typescript
   import express, { Request, Response } from 'express';

	const app = express();
	const PORT = 5000;
	
	app.get('/api/hello', (req: Request, res: Response) => {
	  res.json({ message: 'Hello from Express!' });
	});
	
	app.listen(PORT, () => {
	  console.log(`Server is running on http://localhost:${PORT}`);
	});

   ```
   
4. **Käynnistä palvelin**:
   ```bash
   
   npx ts-node src/index.ts
   
   ```
   
5. Tai vaihtoehtoisesti voit lisätä `nodemon`-käynnistysskriptin `package.json`-tiedostoon:
	```json
 
   	"scripts": {
	  "start": "nodemon src/index.ts"
	}

   ```
   
6. Käynnistä palvelin komennolla:
   ```bash
   
   npm start
   
   ```

---

### Vaihe 3: Frontendin ja backendin yhdistäminen

1. **Muokkaa React-sovellustasi** ja lisää yksinkertainen komponentti, joka hakee datan Express-palvelimelta:
   ```typescript
   import { useEffect, useState } from 'react';

	function App() {
	  const [message, setMessage] = useState<string>('');
	
	  useEffect(() => {
	    fetch('http://localhost:5000/api/hello')
	      .then(response => response.json())
	      .then(data => setMessage(data.message))
	      .catch(error => console.error('Error fetching data:', error));
	  }, []);
	
	  return (
	    <div className="min-h-screen bg-base-100 flex items-center justify-center">
	      <div className="text-center">
	        <h1 className="text-4xl font-bold">React + Express Demo</h1>
	        <p className="mt-4">{message}</p>
	      </div>
	    </div>
	  );
	}
	
	export default App;

   ```
   
2. **Käynnistä sekä frontend että backend**:
   - Varmista, että Express-palvelin on käynnissä `server`-kansiossa.
   - Käynnistä React-sovellus komennolla `npm run dev` sovelluksen `my-app`-kansiossa.

3. **Testaa sovellusta** avaamalla selaimessa `http://localhost:5173` (Vite kehityspalvelimen osoite). React-sovelluksen pitäisi näyttää viestin, joka tulee Express-palvelimelta.

---

### Tehtävän viimeistely

Kun olet saanut yhteyden toimimaan, voit muokata sovellusta lisäämällä uusia reittejä Express.js-palvelimelle ja näyttämällä niiden tietoja Reactin käyttöliittymässä. Voit myös lisätä lisää DaisyUI-komponentteja käyttöliittymän tyylittelyyn.

#### Lisätehtävä:

- Lisää uusi API-reitti, joka palauttaa listan esineitä (esim. kirjat, elokuvat, tuotteet) ja näytä lista React-komponentissa.
- Tyylittele lista käyttäen DaisyUI-komponentteja.

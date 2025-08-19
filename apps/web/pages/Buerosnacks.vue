<template>
  <div class="mx-auto">
    <LandingPageHeader 
      subtitle="​​Snacks für viele – unkompliziert versorgen, vielseitig kombinieren"
      description="Wenn viele Menschen im Haus sind, braucht es Snacks, die einfach funktionieren. Ob Büro, Schule oder Betrieb – gefragt sind Produkte, die schnell bereitstehen, flexibel einsetzbar sind und gern genommen werden.<br>Mit Marken wie <a class='font-bold' href='https://b2b.kelloggs-shop.de/snacks/pringles'>Pringles®</a> und <a href='https://b2b.kelloggs-shop.de/cheezit' class='font-bold'>Cheez-It®</a> Snap’d wird aus dem Pausenraum ein Snackmoment – ganz ohne großen Aufwand."
      buttonText="Zum Snack-Angebot"
      buttonLink="/snacks/pringles/"
      noMarginTop
      headerTop
    />

    <LandingPagePresentation imageSrc="/images/Buerosnacks/PringlesAutomat.jpg" 
        Title="Praktisch im Handling – beliebt im Automaten" 
        Description="Im Snackautomaten zählt nicht nur, was da ist, sondern was auch genommen wird.<br>
                    <a class='font-bold' href='https://b2b.kelloggs-shop.de/snacks/pringles'>Pringles®</a> im bekannten Dosenformat bietet genau das: kompakt, hygienisch, unkompliziert.<br>
                    <a href='https://b2b.kelloggs-shop.de/cheezit' class='font-bold'>Cheez-It®</a> Snap’d ergänzt das Ganze mit ofengebackener Knusprigkeit und würzigem Geschmack. Beide Produkte sind:<br>
                    • Lagerfreundlich<br>
                    • Portionierbar<br>
                    • Ohne Kühlung haltbar<br>
                    Perfekt für Automaten, Auslagen oder Snackstationen.<br>"
        noButton
        noPadding
        />
    <LandingPagePresentation imageLeft :bgColor="'bg-gray-100'" imageSrc="/images/Buerosnacks/Cheezit.png" 
        Title="Verlässliche Auswahl für alle, die zwischendurch eine Pause brauchen" 
        Description="Feste Pausenzeiten? Gibt’s nicht überall. Umso wichtiger ist eine Versorgung, die mit dem Alltag mitgeht.<br>
                    Die Lösung: Snacks, die rund um die Uhr bereitstehen, unabhängig von Ort oder Uhrzeit.<br>
                    Ob früh morgens, zwischen Schichten oder am Wochenende – <a class='font-bold' href='https://b2b.kelloggs-shop.de/snacks/pringles'>Pringles®</a> und <a href='https://b2b.kelloggs-shop.de/cheezit' class='font-bold'>Cheez-It®</a> Snap’d sind immer einsatzbereit." 
        noButton
        noPadding
        />
    

    <LandingPagePresentation imageSrc="/images/Buerosnacks/MrP.jpg" 
      Title="Vielfalt, die mitkommt – für alle Zielgruppen geeignet" 
      Description="Durch ihre Bekanntheit in Deutschland oder der USA*, einfache Handhabung und breite <br>
                    Geschmacksvielfalt eignen sich diese Snacks besonders für den Einsatz in großen Teams, öffentlichen Einrichtungen oder bei Events.<br>
                    Was sie besonders macht:<br>
                    • Kein zusätzlicher Aufwand für Kühlung oder Zubereitung<br>
                    • Breite Geschmackspalette – von mild bis würzig<br>
                    • Gute Sichtbarkeit und Wiedererkennung<br>
                    Jetzt Sortiment erweitern – mit Snacks, die bleiben."
      Additional="* Quelle: NIQ Brand Health Tracker 2024"
      noButton
      noPadding
      />


  </div>
</template>
<script lang="ts" setup>

const { data: productsCatalog, loading, fetchProducts } = useProducts();
const { isAuthorized } = useCustomer();
import { SfLoaderCircular } from '@storefront-ui/vue';
import { Product } from "@plentymarkets/shop-api";
import { useAsyncData } from 'nuxt/app';

const pringlesProducts = ref([] as Product[]);
const cerealienProducts = ref([] as Product[]);

const { data: pringlesData } = await useAsyncData('pringlesProducts', () => fetchProducts({ categoryUrlPath: 'snacks/pringles', page: 1, itemsPerPage: 4 }))
const { data: cerealienData } = await useAsyncData('cerealienProducts', () => fetchProducts({ categoryUrlPath: 'fruehstueck/cerealien', page: 1, itemsPerPage: 4 }))

// Set initial values from SSR/CSR fetch
if (pringlesData.value?.products) pringlesProducts.value = pringlesData.value.products;
if (cerealienData.value?.products) cerealienProducts.value = cerealienData.value.products;

async function reloadProducts() {
  const pringlesRes  = await fetchProducts({ categoryUrlPath: 'snacks/pringles', page: 1, itemsPerPage: 4 })
  const cerealienRes = await fetchProducts({ categoryUrlPath: 'fruehstueck/cerealien', page: 1, itemsPerPage: 4 })
  pringlesProducts.value  = pringlesRes.products
  cerealienProducts.value = cerealienRes.products 
}

watch(isAuthorized, (newVal) => {
  if (newVal) {
    reloadProducts()
  }
})

const { getRobots, setRobotForStaticPage } = useRobots();


await getRobots();
setRobotForStaticPage('Homepage');

</script>



<style>
.klg-red {
  border-color: #f60b45;
  color: #f60b45;
}

.global-sizes { 
  max-width: 1200px;
  min-height: 355px;
}
</style>
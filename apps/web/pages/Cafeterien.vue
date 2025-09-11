<template>
  <div class="mx-auto">
    <LandingPageHeader 
      subtitle="Pausensnack für Cafeterien – wirtschaftlich & beliebt"
      description="Snackversorgung, die mit dem Alltag Schritt hält Ob Uni, Kantine oder Business Lounge – in der kurzen Pause zählen schnelle Entscheidungen und unkomplizierte Angebote. <br>
                  Snacks wie <a class='font-bold' href='https://b2b.kelloggs-shop.de/snacks/pringles'>Pringles<span class='text-sm align-super'>®</span></a> und <a class='font-bold' href='https://b2b.kelloggs-shop.de/cheezit'>Cheez-It<span class='text-sm align-super'>®</span></a> Snap’d lassen sich leicht in bestehende Sortimente integrieren: bekannte Marken*, kompakte Formate und einfach im Handling. Ideal für Cafeterien, in denen es auf Platz, Tempo und Vielfalt ankommt."

      noMarginTop
      headerTop
    />

    <LandingPagePresentation imageSrc="/images/cafeterien/1.png" 
        Title="Praktisch im Regal. Passend im Automaten." 
        Description="<a class='font-bold' href='https://b2b.kelloggs-shop.de/snacks/pringles'>Pringles<span class='text-sm align-super'>®</span></a> steht für den bekannten Stapelchip im Dosenformat, <a class='font-bold' href='https://b2b.kelloggs-shop.de/cheezit'>Cheez-It<span class='text-sm align-super'>®</span></a>  Snap’d ergänzt das Sortiment mit ultradünnem Käse-Crunch aus dem Ofen.<br>
                    Beide Produkte sind ready-to-go, benötigen keine Kühlung und lassen sich flexibel platzieren <br>
                    – egal ob in der Auslage oder im Automaten. <br>
                    Durch ihre Bekanntheit* erleichtern sie die Kaufentscheidung – und lassen sich unkompliziert nachbestücken."
        noButton
        noPadding
        />
    <LandingPagePresentation imageLeft :bgColor="'bg-gray-100'" imageSrc="/images/cafeterien/2.png" 
        Title="Zwischen Vorlesung und Feierabend – Snacks, die mitkommen" 
        Description="Der Tagesrhythmus vieler Gäste ist flexibel – Snacks müssen das mitmachen.<br>
                    <a class='font-bold' href='https://b2b.kelloggs-shop.de/snacks/pringles'>Pringles<span class='text-sm align-super'>®</span></a> und <a class='font-bold' href='https://b2b.kelloggs-shop.de/cheezit'>Cheez-It<span class='text-sm align-super'>®</span></a> Snap’d sind lange haltbar, schnell zur Hand und geschmacklich vielseitig. Ob als Ergänzung zum Getränk, als kleine Pause zwischendurch oder für unterwegs – sie passen sich verschiedenen Nutzungssituationen an." 
        noButton
        noPadding
        />
    

    <LandingPagePresentation imageSrc="/images/cafeterien/3.jpg" 
      Title="Sortiment erweitern – einfach und effizient" 
      Description="Snacks müssen nicht kompliziert sein, um gut zu funktionieren.<br>
                  Mit <a class='font-bold' href='https://b2b.kelloggs-shop.de/snacks/pringles'>Pringles<span class='text-sm align-super'>®</span></a> und <a class='font-bold' href='https://b2b.kelloggs-shop.de/cheezit'>Cheez-It<span class='text-sm align-super'>®</span></a> Snap’d ergänzt du dein Angebot um Artikel, die sowohl logistisch als auch geschmacklich gut ins Tagesgeschäft passen – ohne großen Aufwand.<br>
                  Jetzt Sortiment entdecken – und gezielt um praktische Snacklösungen erweitern."
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
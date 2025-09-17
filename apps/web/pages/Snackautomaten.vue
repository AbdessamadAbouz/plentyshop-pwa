<template>
  <div class="mx-auto">
    <LandingPageHeader 
      subtitle="Snackautomaten für Büro, Pausenraum und Werkstatt - Sortiment & Service"
      description="Smarte Snacklösung für den Pausenraum Snacks bereitstellen, ohne Aufwand? Genau dafür gibt’s den <a class='font-bold' href='https://b2b.kelloggs-shop.de/snacks/b2b-starterkits/'>Pringles<span class='text-sm align-super'>®</span> Dispenser.</a> Einfach aufstellen, 40 g Dosen einlegen – das war’s. Kein Strom, kein System, keine. Einweisung. Der Dispenser eignet sich für Büros, Werkstätten und Sozialräume – überall dort, wo Snacks unkompliziert griffbereit sein sollen. Ein Handgriff, ein kurzer Moment Pause – mehr braucht’s nicht."
      noMarginTop
      headerTop
    />

    <LandingPagePresentation imageSrc="/images/snackautomaten/1.png" 
        Title="Wertschätzung zeigen – ganz nebenbei" 
        Description="Der <a class='font-bold' href='https://b2b.kelloggs-shop.de/snacks/b2b-starterkits/'>Pringles<span class='text-sm align-super'>®</span> Dispenser</a>. bringt nicht nur Snacks in Reichweite – sondern macht sichtbar: Hier wird an die Pause gedacht. Dank auffälligem Design zieht er Blicke auf sich, das Handling bleibt dabei simpel. Ob neben dem Kaffeeautomaten, in der Teeküche oder am Flurende – der Dispenser passt sich an."
        noButton
        noPadding
        />
    <LandingPagePresentation imageLeft :bgColor="'bg-gray-100'" imageSrc="/images/snackautomaten/2.jpg" 
        Title="Ein System, das mitläuft" 
        Description="Stabil, kompakt, hygienisch: Der <a class='font-bold' href='https://b2b.kelloggs-shop.de/snacks/b2b-starterkits/'>Pringles<span class='text-sm align-super'>®</span> Dispenser</a> funktioniert ohne Technik oder Wartung. Die 40 g Dosen lassen sich schnell nachlegen, ganz ohne Kühlaufwand. So entsteht ein Snackangebot, das flexibel bleibt – und sich in viele Alltagssituationen integrieren lässt." 
        noButton
        noPadding
        />
    

    <LandingPagePresentation imageSrc="/images/snackautomaten/3.jpg" 
      Title="Kleine Geste, großer Effekt" 
      Description="Ein Griff zur Dose – mehr braucht es oft nicht für einen kurzen Snackmoment. Der Dispenser bringt knusprige Abwechslung genau dahin, wo Menschen zusammenkommen – sichtbar, zugänglich, bereit. Ob fürs Homeoffice-Team, den Schichtbetrieb oder das Büro mit Laufkundschaft: So wird Snackversorgung einfach gemacht – alltagstauglich, markenstark und ohne Extraaufwand. Jetzt entdecken und Pausenräume unkompliziert ausstatten."
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
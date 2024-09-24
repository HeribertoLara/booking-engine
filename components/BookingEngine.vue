<template>
  <div>
    <form @submit.prevent="handleSubmit">
      <!-- Hotel Selection -->
      <SelectHotel
        :hotels="currentHotelOptions"
        v-model="selectedHotel"
        :is-flight-included="isFlightIncluded"
      />

      <!-- Toggle Flight Inclusion -->
      <section>
        <!--  debemos enviar  airport  como props  debemos estar atentos al cambio en el estado de  airport  este cambio es emitido por  select airport -->
        <ToggleFlight
          v-model="isFlightIncluded"
          @update:is-flight-included="updateFlightInclusion"
          @toggle-open-modal="handleToggleOpenModal"
          :airport="airport"
        />

        <!-- SelectAirport visible solo si isFlightIncluded es true -->
        <SelectAirport
          v-model:isOpenModal="isOpenModal"
          v-model:airport="airport"
          @update:airport="updateAirport"
        />
      </section>

      <!-- Stay Dates -->
      <DateSelector
        v-model:modelValueArrival="arrivalDate"
        v-model:modelValueDeparture="departureDate"
      />

      <!-- Guests Section -->
      <Guests
        v-model:adults="adults"
        v-model:numberChildren="numberChildren"
        v-model:childrenAges="childrenAges"
      />

      <!-- Promo Code Section -->
      <PromoCode v-model:promoCode="promoCode" />


      <!-- Submit Button -->
      <button type="submit">Book Now</button>
    </form>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import SelectHotel from "./SelectHotel.vue";
import ToggleFlight from "./ToggleFlight.vue";
import DateSelector from "./DateSelector.vue";
import Guests from "./Guests.vue";
import PromoCode from "./PromoCode.vue";
import SelectAirport from "./SelectAirport.vue";
import { hotelOptions } from "@/data/hotelOptions";
import { reservHotelOptions } from "@/data/reservHotelOptions";
import { addDays } from "date-fns";

// Estado del formulario
const selectedHotel = ref("");
const isFlightIncluded = ref(false);
const isOpenModal = ref(false); // Nueva variable para manejar la visibilidad del modal
const adults = ref(2);
const numberChildren = ref(0);
const childrenAges = ref([]);
const promoCode = ref("");
const airport = ref({ label: "", value: "" });
const today = new Date();
const arrivalDate = ref(addDays(today, 7)); // 7 días después de hoy
const departureDate = ref(addDays(today, 12)); // 12 días después de hoy

// URL Base para la reserva
const urlBase = "https://booking.example.com";

// Opciones de hoteles basadas en la inclusión de vuelo
const currentHotelOptions = computed(() => {
  return isFlightIncluded.value ? reservHotelOptions : hotelOptions;
});

// Actualizar el estado de inclusión de vuelo desde el ToggleFlight
const updateFlightInclusion = (value) => {
  isFlightIncluded.value = value;
};

// Maneja la apertura del modal cuando se activa el toggle
const handleToggleOpenModal = () => {
  isOpenModal.value = true;
};

// Función para cerrar el modal manualmente
const handleCloseModal = () => {
  isOpenModal.value = false;
};

/* // Función para construir la URL de reserva
const constructBookingURL = (params) => {
  const {
    withFly,
    reservHotel,
    hotel,
    arrivalDate,
    departureDate,
    adults,
    childAges,
    promoCode,
    urlBase,
  } = params;

  let url = withFly
    ? `${urlBase}/${reservHotel.location}/${reservHotel.value}/booking-engine/ibe5.main?hotel=${reservHotel.noHotel}&aDate=${arrivalDate}&dDate=${departureDate}&adults=${adults}&child=${childAges.length}&rooms=1&source=&show_ta_comm=&agent_fee=&abtest=&aff=&currency=&agent=&usr=&lang=1&showHotel=&rategroup=&rate=&sub_source=&PCC=&AirportDep=${airport.value}&PC=${promoCode}&view_type=&groupId=`
    : `${urlBase}/${hotel.value}/${arrivalDate}/${departureDate}/${adults}`;

  if (childAges.length > 0) {
    const childrenAges = childAges.join(withFly ? "&childages=" : ";");
    url += withFly ? `&childages=${childrenAges}` : `;${childrenAges}`;
  }

  if (promoCode) {
    url += withFly ? `&cp=${promoCode}` : `?cp=${promoCode}`;
  }

  return url;
};
 */
// Función de envío del formulario
const handleSubmit = (e) => {
  e.preventDefault();

  console.log(":D este es el envio del form");
  /* try {
    const isValidHotel = validateHotelSelection();
    const isValidDates = validateDates();
    const isValidAdults = validateAdults();
    const isValidChildren = validateChildrenAges();

    if (!isValidHotel || !isValidDates || !isValidAdults || !isValidChildren)
      return;

    const dates = formatAndValidateDates();
    if (!dates) return;

    const bookingUrl = constructBookingURL({
      withFly: isFlightIncluded.value,
      reservHotel: selectedHotel.value,
      hotel: selectedHotel.value,
      arrivalDate: dates.formattedArrivalDate,
      departureDate: dates.formattedDepartureDate,
      adults: adults.value,
      childAges: childAges.value,
      promoCode: promoCode.value,
      urlBase: urlBase,
    });

    window.open(bookingUrl, "_blank");
  } catch (error) {
    console.log(error);

  } */
};

const updateAirport = (selectedAirport) => {
  airport.value = selectedAirport; // Asegura que sea un objeto completo {label, value}
  console.log(
    `Airport updated: ${airport.value.label} - ${airport.value.value}`
  );
};

watch([adults, numberChildren, childrenAges, airport, ], ([newAdults, newNumberChildren, newChildrenAges, newAirport]) => {
  console.log('Adults:', newAdults);
  console.log('Number of Children:', newNumberChildren);
  console.log('Children Ages:', newNumberChildren ? newChildrenAges : []);
  console.log('AirportSelected:', newAirport );
});
</script>

<style >
/* Usa el archivo BookingEngine.scss para tus estilos */
</style>

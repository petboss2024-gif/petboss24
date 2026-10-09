# Convenciones

## Nombres: lowerCamelCase

Primera palabra en minúscula, las siguientes con mayúscula inicial, sin
espacios, guiones bajos ni emojis. Prefijo `os` para campos y option sets que
referencian un option set; `nav` para navegación.

Ejemplos: `osRole`, `navStatusRequest`, `listOfPets`, `seenCustomer`.

Solo se cambian **nombres visibles** (Bubble mantiene la clave interna, así que
workflows y datos no se rompen). Las **páginas web** no siguen esta regla: su
nombre es la URL y Bubble solo admite minúsculas.

## Estado del renombrado

### Hecho (2026-10-09)

Campos de tipos de datos:

| Tipo | Antes → Ahora |
|---|---|
| User | OS_role → `osRole`; OS_statusVerificationPetsitter → `osStatusVerificationPetsitter` |
| address | OS_accessMethod → `osAccessMethod`; User → `user` |
| profile | OS_gender → `osGender` |
| dgDisponibility | Services → `services` |
| dgRequest | nav_schedule → `navSchedule`; nav_StatusRequest → `navStatusRequest`; Addtime → `addTime`; os_tip → `osTip`; os_dgType → `osDgType`; listOfPetses → `listOfPets` |
| dgSubServices | os_dgtype → `osDgType` |
| requestAddTime | OS_addTime → `osAddTime` |
| feedback | os_feedbackCategory → `osFeedbackCategory`; Stars → `stars` |
| messages | message_date → `messageDate`; seen customer → `seenCustomer`; seen petsitter → `seenPetsitter` |
| support | type_support → `typeSupport` |
| transaction | paymenStatus → `paymentStatus` |

Reusables y vistas: `header`, `footer`, `offlineBanner`, `updateApp`.

### No posible por herramienta

- **Device** (tipo nativo de Bubble).
- **Option sets y sus atributos**: el conector de Bubble no permite
  renombrarlos. Hacerlo a mano en *Data → Option sets* (es seguro).

### Checklist manual de option sets

- [ ] `➡️ Nav_General` → `navGeneral`
- [ ] `➡️ Nav_Schedule` → `navSchedule`
- [ ] `➡️ Nav_Petsitter` → `navPetsitter`
- [ ] `➡️ Nav_Logged_Out` → `navLoggedOut`
- [ ] `➡️Nav_Help_Support` → `navHelpSupport`
- [ ] `➡️Nav_Status_Request` → `navStatusRequest`
- [ ] `➡️ Nav_About_Customer` → `navAboutCustomer`
- [ ] `➡️ Nav_Service_DogWalking` → `navServiceDogWalking`
- [ ] `➡️ Sub_Nav_Profile` → `subNavProfile`
- [ ] `Sub_Status_Booked` / `Sub_Status_Pending` / `Sub_Status_Past` → `subStatusBooked` / `subStatusPending` / `subStatusPast`
- [ ] `OS_Role`, `OS_Size`, `OS_Gender`, `OS_ID`, `OS_Rates` → `osRole`, `osSize`, `osGender`, `osId`, `osRates`
- [ ] `OS_Markers`, `OS_Markers_Type` → `osMarkers`, `osMarkersType`
- [ ] `OS_Metrics`, `OS_Onb_Pet`, `OS_Sort_By`, `OS_Onboarding` → `osMetrics`, `osOnbPet`, `osSortBy`, `osOnboarding`
- [ ] `OS_Review_Pet`, `OS_Review_Petsitter`, `OS_Review_Customer` → `osReviewPet`, `osReviewPetsitter`, `osReviewCustomer`
- [ ] `OS_Social_Media`, `OS_Spayed/Neutered`, `OS_Info_Calendar` → `osSocialMedia`, `osSpayedNeutered`, `osInfoCalendar`
- [ ] `OS_Filter_By_Service`, `OS_Filter_Date_Metrics` → `osFilterByService`, `osFilterDateMetrics`
- [ ] `OS_Frequently_Asked_Questions` → `osFrequentlyAskedQuestions`
- [ ] `OS_Status_Verification_Petsitter` → `osStatusVerificationPetsitter`
- [ ] `OS_Visible_Pet_Invormation` → `osVisiblePetInformation`
- [ ] `What_Our_Users_Say` → `whatOurUsersSay`
- [ ] `Get_the_Petboss_24_App` → `getThePetboss24App`
- [ ] `carrouselInfo` → `carouselInfo`

Atributos (ejemplos): `Description` → `description`, `Image` → `image`,
`Icon_Selected` → `iconSelected`, `image no selected` → `imageNoSelected`,
`time/min` → `timeMin`, `Coming soon` → `comingSoon`, `Img Active` →
`imgActive`, `Sub text` → `subText`, `TYPE` → `type`, `URL` → `url`,
`CTA` → `cta`, `ID` → `id`.

## Colores y tipografía

- Primario `#FF6600`, fondo `#161616`, superficies `#2C2C2C`.
- Texto secundario `#8A8A8A` / `#B3B3B3`.
- Poppins para UI, DM Mono para números.
- Layout mobile-first, máx. 430px de ancho.

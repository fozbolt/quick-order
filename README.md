**Force push from our original repo: https://github.com/mstjepan28/quick-order

Naslov projekta: Quick order

Izradili: Kristijan Krulić, Filip Ožbolt i Stjepan Markovčić

Opis projekta:
Aplikacija Quick order je napravljena da olakša i ubrza cjelokupni proces od naručivanja do dobivanja narudžbe na stol u restoranu. Njenu funkcionalnost bi bilo najlakše opisati tako da je usporedimo s tabletima koje imaju konobari u nekim restoranima gdje u trenutku zaprimanja narudžbe mušterije on direktno unosi narudžbu u tablet i ona se šalje barmenu te ju barmen pripremi bez da konobar mora svaki put prići do njega i reći mu svaku narudžbu. Naša aplikacija sadrži taj element, ali i mnoge druge koji predstavljaju znatnije poboljšanje i ubrzanje usluge u restoranu. Možemo reći da ova aplikacija predstavlja revoluciju odnosno evoluciju postojeće. Quick order se sastoji od 5 sučelja(mušterija, konobar, kuhar, barmen, menadžer). Navesti ćemo poboljšanja a zatim ih detaljnije pojasniti u 3. poglavlju

Komponente: 
-Vue Croppa (https://zhanziyang.github.io/vue-croppa/?fbclid=IwAR2Y0BWJLIp9M2GVJRPmaOuR3v-GlE03w4669qE7NwVgO0Q0XuwTVXwAZKc#/)
-Current Date & Time (https://tecadmin.net/get-current-date-time-javascript/?fbclid=IwAR3tQwcRhAIlm1H6naPRv_r0Sm7lq4-_DqcpB7lYFcvonegBHLlgzWuc_2Y, https://stackoverflow.com/questions/1531093/how-do-i-get-the-current-date-in-javascript?fbclid=IwAR3kgUTMcun3IcoWh816gqCQrliEADzezRPkrjMqUf7YHt9ojkNGfeSz6q4)
-Bootstrap (https://getbootstrap.com/docs/4.0/components/modal/, https://getbootstrap.com/docs/4.0/components/navs/)
-Font Awesome (https://fontawesome.com/)

Poboljšanja: - Mušterija sama naručuje ako želi (ne mora čekati konobara da dođe)
		-Konobar naručuje preko aplikacije(ne samo barmenu nego i kuharu)
		-Kuhar i barmen potvrđuju/zaprimaju narudžbu i šalju obavijest konobaru kad je gotova (Konobar ne treba nasumice provjeravati u kuhinju)
		-Mušterija ima uvid u status narudžbe (vidi gdje je „zapelo“)
		-Mušterija „pušta“ feedback o namirnicama i usluzi
		-Menadžer dodaje u meni jela i pića te korisnike/zaposlenike u sustav
		-Menadžer ima uvid o potrošnju/potražnju i feedback o namirnicama
		-Mušterija može na klik gumba pozvati konobara ako ga nema u blizini



Bitno je spomenuti da je korisnicima aplikacije pojednostavljeno i ubrzano korištenje aplikacije i u smislu da nema potrebe da se mušterija registrira, a zatim logira u aplikaciju da bi mogla naručiti hranu, nego svaki stol u restoranu sadrži QR kod kojeg mušterija jednostavno skenira i njezina narudžba je identificirana pod određeni broj stola. Svjesni smo da ovaj način ima sigurnosnih propusta i aplikaciju ćemo nadograditi sa sigurnosnog aspekta u idućim verzijama. Korištene tehnologije: Vue.js, Javascript, Firebase, HTML , CSS.

Login info: 
E-Mail/Password | konobar@gmail.com 1234567890 | kuhar@gmail.com 1234567890 barmen@gmail.com 1234567890 | menadzer@gmail.com 1234567890

# CrazyTrader.ai v02 — Raport de actualizare a documentației

Data: 4 octombrie 2026. Destinatar: proprietarul aplicației. Bază comparată: commit `6a437ac2d745e2c901e8c46ec483211721cb47cf`, 40 de fișiere Markdown. Obiectul lucrării: documentație și plan de implementare, fără cod de aplicație, tranzacții, chei sau instalări.

## 1. Rezultatul și limita exactă a lucrării

Documentația a fost restructurată și corelată pentru ținta Enterprise Local: o aplicație completă pentru un singur operator, cu două moduri de tranzacționare, agenți permanenți reali, Loki, integrare Claude CLI prin abonamentul proprietarului, controale financiare independente de AI și progresie verificabilă până la tranzacționare reală.

Au fost păstrate toate cele 40 de căi originale. Revizia conține 54 de fișiere Markdown: 33 originale modificate, 7 originale păstrate identic și 14 adăugate. Nu a fost șters niciun fișier original. Auditul independent complet a fost inclus separat, cu conținutul său original, pentru trasabilitate. Inventarul de la final precizează rolul fiecărui fișier.

Plusul adus este trecerea de la principii și liste de componente la contracte mult mai concrete: ce date intră, cine are autoritate, cum se păstrează capitalul, ce se întâmplă la erori, ce consumă abonamentul și ce dovezi permit trecerea la etapa următoare. Nu am transformat însă specificațiile în capabilități executabile prin simpla lor redactare.

Starea reală rămâne L0 DEVELOPMENT. Nu există încă aplicație funcțională, agenți executabili, modele validate, rezultate de backtest, integrare Claude autentificată, infrastructură pornită sau probe de tranzacționare reală. Niciuna dintre aceste lipsuri nu este ascunsă prin formularea „documentație finalizată”.

## 2. Ce am înțeles și fixat din cerințele tale

Enterprise Local este produsul complet pentru tine, nu o demonstrație care devine utilizabilă abia după SaaS. Trebuie să poți folosi ambele moduri, să vezi capitalul și rezultatele separat, să înțelegi deciziile, să controlezi riscul și să tranzacționezi real numai după validare. Intenția de a finanța dezvoltarea ulterioară din eventualele câștiguri este păstrată ca obiectiv economic al proprietarului, fără promisiune de venit sau calendar de profit.

Calculele matematice, semnalele, verificările de risc, contabilitatea și protecția pozițiilor rulează în aplicație. Ele folosesc resurse locale și conectivitate/API de exchange, dar nu cer apeluri LLM pentru fiecare calcul. Agenții există permanent ca identitate, responsabilitate, memorie și istoric. Sunt activați pentru sarcini justificate; nu sunt unsprezece modele care conversează continuu.

Claude Code CLI oficial, nemodificat, cu autentificarea nativă a proprietarului, este ruta AI cerută pentru acceptarea produsului local. Documentele cer o probă reală pe calculatorul și abonamentul tău. Nu presupun că abonamentul este un API nelimitat și nu autorizează copierea tokenurilor într-un proxy. Limitele abonamentului sunt comune agenților. Lipsa cotei suspendă sarcinile AI; nu oprește contabilitatea, reconcilierea sau protecția deterministă.

Loki este al unsprezecelea agent permanent. El explică aplicația, starea ei reală, motivele unei tranzacții sau ale unui refuz, incidentele și pașii disponibili. Consultă documentația versionată și surse operaționale permise. Preferă cel mai economic model disponibil în abonament care trece evaluările; disponibilitatea Haiku nu este inventată ca fapt. Loki nu primește secrete și nu poate transforma o conversație în ordin de tranzacționare ori majorare de capital.

## 3. Ce a rămas din arhitectura originală

Au fost păstrate deciziile bune, nu doar numele componentelor:

- Separarea AI de autoritatea financiară: agenții cercetează și explică; controalele deterministe autorizează și execută.
- Math Mode și Strategy Mode, cu bugete, P&L, drawdown și comenzi Auto Trading distincte.
- Hard Risk separat de Risk Analyst Agent și OPA după evaluarea riscului.
- Tratarea ordinului cu răspuns necunoscut ca UNKNOWN, fără retrimitere oarbă.
- Registrul contabil append-only, corecții compensatorii și reconciliere cu exchange-ul.
- Versiuni imuabile ale strategiilor/modelelor, promovare controlată și imposibilitatea autopromovării agenților.
- Identitatea permanentă a agenților independentă de modelul folosit.
- Secretelor live le este interzis accesul în cloud-ul de dezvoltare și în contextul LLM.
- Excepția L5 de canary limitat înaintea L6, cu activare explicită locală; problema veche L5/L6 era deja rezolvată și nu a fost prezentată ca descoperire nouă.
- Lipsa promisiunilor de randament și acceptarea situației în care sistemul nu tranzacționează.
- Utilizarea componentelor mature open source și separarea fazei globale SaaS.
- ExecPlan, checkpoint-uri și reluare onestă după limitele instrumentului de dezvoltare.

Fișierele istorice de audit/proveniență nu au fost rescrise pentru a sugera că vechea versiune avea deja noile contracte.

## 4. Unde este îmbunătățirea concretă

### 4.1 Produs și autoritate documentară

PRODUCT_REQUIREMENTS definește rezultatul final și probele necesare pentru fiecare capabilitate. Cele trei ADR-uri noi decid: arhitectura locală modulară, agenții declanșați de evenimente și Loki/ruta Claude, respectiv autoritatea financiară atomică și certificarea pe domeniu exact. README, AGENTS, prompturile de pornire și STATUS au fost aliniate.

Am separat trei întrebări care înainte se puteau confunda: funcționează corect infrastructura financiară; este eligibilă economic o strategie concretă; este complet produsul cerut? Un motor sigur poate exista fără strategie eligibilă. Un backtest bun nu justifică un motor nesigur. Produsul complet nu se declară terminat cât timp lipsește o cerință obligatorie.

### 4.2 Capitalul nu poate fi „cheltuit de două ori” prin concurență

FINANCIAL_AUTHORIZATION definește o rezervare atomică a capitalului, legată de intenție, cont, portofoliu, configurație, politică, expirare și versiunea stării. Verificarea riscului nu mai este un răspuns vechi reutilizabil după schimbarea situației. Autorizația este consumată o singură dată de expeditorul autorizat.

Exemplul pe care implementarea va trebui să îl respingă: Math și Strategy văd simultan 100 USDT disponibili și cer fiecare 60. Aprobările individuale nu pot permite în total 120. Același control include ordine deschise, rezervări, taxe și ordine UNKNOWN. Kill este verificat înainte de transmitere; un ordin deja pe rețea poate totuși ajunge la exchange și trebuie reconciliat. Nu am promis atomicitate imposibilă între baza locală și Binance.

### 4.3 Ordinele și contabilitatea au semnificații mai precise

Am separat intenția economică, cererea de execuție, acțiunea logică și încercarea de rețea. O eroare de transport nu justifică o nouă identitate economică. Un singur răspuns NOT_FOUND nu dovedește că ordinul nu a fost acceptat.

Mașina de stări acoperă fill înainte de ACK, fill în timpul anulării, anulare parțială, evenimente duplicate și întârziate. Cantitatea executată nu poate regresa când sosește un mesaj vechi. Rezervarea nu este eliberată doar fiindcă a expirat un cronometru local.

Ledger-ul are contabilizare echilibrată separat pe fiecare activ, taxe în activul real, transferuri explicite între moduri, cont pentru diferențe neatribuite și actualizare atomică a fill-ului, inventarului, rezervării și evenimentelor. Depozitele și transferurile nu devin profit al strategiei. Evenimentele externe manuale sunt detectate, nu ignorate.

### 4.4 Urgențele nu mai depind de promisiuni imposibile

RISK_SECURITY conține matricea de comportament la căderea Claude, datelor, Hard Risk, OPA, bazei de date, secretelor, rețelei, exchange-ului sau calculatorului. Calea de reducere este deterministă, limitată și preautorizată, cu stare verificabilă și jurnal durabil.

Dacă exchange-ul nu răspunde sau calculatorul nu are curent, aplicația nu poate garanta închiderea poziției. Dacă starea și jurnalul nu sunt de încredere, nu inventează o cantitate de vândut. Se păstrează alertele, procedura pentru proprietar și reconcilierea ulterioară. Protecția nativă de exchange contează numai când acel tip de ordin este implementat, validat și confirmat.

### 4.5 Cele două metode sunt acum specificate ca ipoteze testabile

Originalul definea două moduri, nu două algoritmi compleți. Am introdus MATH-RIDGE-01: un model de regresie regularizată pentru randament pe orizont fix, cu caracteristici, etichete, scalare numai pe datele de antrenare, costuri și reguli de dimensionare/ieșire. Am introdus STRATEGY-DONCHIAN-01: breakout long-only cu canal calculat din bare anterioare, filtru de trend, stop și ieșire temporală/canal.

Ambele au convenții explicite pentru momentul disponibilității datelor și întârzierea intrării, pentru a evita cumpărarea fictivă la un preț deja trecut. Sunt candidate inițiale de cercetare, nu strategii declarate profitabile sau validate. Dacă sunt respinse de experimente, rezultatul se păstrează și se formulează o ipoteză nouă.

A dispărut exemplul de „confidence 0.84” fără definiție. O probabilitate trebuie să aibă eveniment, orizont și calibrare; altfel câmpul se omite. Randamentul estimat, costul, incertitudinea și probabilitatea nu mai sunt amestecate. Calculele statistice pot folosi float64 controlat, iar banii și conversiile pentru ordine folosesc Decimal.

### 4.6 Cercetarea poate respinge idei, nu doar produce grafice frumoase

Protocolul cere separare cronologică, walk-forward, eliminarea suprapunerii etichetelor între seturi, registrul tuturor încercărilor, ajustarea testărilor multiple, bootstrap care păstrează dependența temporală, stres de cost/lichiditate/latency și comparație cu repere simple. Alegerea parametrilor după observarea rezultatelor nu mai poate fi prezentată drept test neatins.

Mărimea eșantionului se justifică prin precizie/putere și observații efectiv independente, nu printr-un număr universal de tranzacții. Regimurile neobservate rămân dovezi insuficiente. Learning primește și abțineri, refuzuri și ordine neexecutate, nu numai tranzacții selectate de politica veche. Rezultatele ipotetice sunt separate de P&L real.

### 4.7 Agenții au contract operațional și buget

AGENT_RUNTIME definește sarcini persistente, stări, lease, checkpoint, idempotentă, deadline, deduplicare, agregare, cooldown, priorități și limită de concurență. Valorile inițiale sunt conservatoare și trebuie măsurate. O explicație AI poate aștepta; o oprire de risc nu intră în aceeași coadă.

Au fost păstrate cele zece responsabilități originale și adăugat Loki. Pentru fiecare sunt definite declanșatorul, intrările, rezultatul și autoritatea interzisă. Funcțiile mecanice rămân software: detectorul de regim live, verificarea limitelor, rutarea evenimentelor și recuperarea ordinelor nu sunt delegate improvizației LLM.

Un agent este acceptat numai dacă are instrumente reale, date și istoric persistente, recuperare și evaluări. Un răspuns textual plauzibil nu dovedește că a executat o sarcină. Loki are o suită inițială de minimum 60 de cazuri și praguri pentru răspunsuri fundamentate, refuz corect și protejarea acțiunilor sensibile.

### 4.8 Claude prin abonament este o cerință verificabilă

AI_PROVIDER_ROUTING separă ruta CLI nativă, instrumentele de dezvoltare, SDK/API comercial și alternativele locale. CLAUDE_SUBSCRIPTION_SPIKE cere dovezi despre versiune, model efectiv, autentificare, izolare, instrumente încărcate, cotă, timeout, reluare și lipsa fallback-ului plătit. Nicio autentificare reală nu a fost efectuată în această lucrare.

Documentația oficială consultată este legată în specificație și datată. Particularitatea importantă pentru implementare este că izolarea trebuie verificată pentru ruta de abonament; o opțiune CLI care cere cheie API nu devine automat soluția corectă. Condițiile furnizorului și disponibilitatea modelelor trebuie reverificate la integrare și upgrade.

Cost suplimentar LLM zero este profilul de rutare dorit, nu o afirmație că operarea nu costă nimic. Rămân abonamentul existent, hardware, energie, stocare, conectivitate, taxe și spread/slippage de tranzacționare. Cota poate limita cercetarea și răspunsurile agenților.

### 4.9 Enterprise local fără infrastructură inutilă

Am păstrat modulele și responsabilitățile, dar am eliminat obligația de a porni din prima câte un serviciu pentru fiecare. Baza este un nucleu financiar modular, PostgreSQL, OPA local, UI/API, cercetare izolată și procese AI izolate. Parquet și DuckDB pot acoperi analiza locală inițială. NATS, ClickHouse, MLflow server și object storage server devin opțiuni motivate de măsurători.

NautilusTrader rămâne candidatul principal pentru reutilizarea execuției/simulării, dar trebuie demonstrat că există un singur proprietar al stării ordinelor și un singur expeditor. SDK-ul Binance nu poate crea o rută paralelă necontrolată. Licențele și versiunile se verifică înainte de integrare; în acest pas nu au fost instalate dependențe.

### 4.10 Producția locală are porți măsurabile

Certificarea este legată de build, strategie/model, date, costuri, configurație, exchange, cont, instrumente și tipuri de ordine. Schimbările materiale invalidează probele afectate. L6 este nivel intern al proiectului, nu certificare de la un regulator sau garanție de profit.

Paper și shadow au praguri operaționale inițiale de minimum 14 zile observate fiecare, cu suprapunere declarată posibilă. Aceste praguri nu sunt dovadă statistică suficientă. Canary are criterii stabilite înainte de activare, proporționale cu frecvența strategiei și capitalul permis. Nu se simulează timpul trecut și nu se forțează semnale.

Au fost introduse ținte inițiale de măsurat pentru Kill, alerte, RPO de dezastru de maximum 15 minute și RTO până la stare sigură reconciliată de maximum 30 minute. Nu sunt rezultate obținute. Restaurarea pornește fără transmitere de ordine, iar reactivarea live nu este automată. Două calculatoare cu aceeași cheie Binance nu devin sigure doar printr-un lease în baza de date.

## 5. Trasabilitatea celor 24 de constatări

Pentru toate rândurile, starea este „corecție documentară definită; dovada executabilă rămâne de produs”. Severitatea este cea din auditul inițial, nu un incident descoperit într-un program care rulează.

| ID / severitate | Plusul documentar și fișierul principal | Ce mai trebuie demonstrat |
|---|---|---|
| F01 BLOCKER | QUANTITATIVE_METHODS: doi candidați compleți și ipoteze explicite | Formule verificate, date și experimente; eligibilitate poate fi respinsă |
| F02 CRITICAL | TRADE_INTENT / RISK_SECURITY: unități, edge net, confidence și profile | Scheme, calibrare dacă se folosește probabilitate, configurații numerice |
| F03 BLOCKER | FINANCIAL_AUTHORIZATION: rezervare și autorizație atomică | Concurență, expirare, Kill și crash la fiecare frontieră |
| F04 CRITICAL | RISK_SECURITY: matrice de dependențe și jurnal de urgență | Reducere sigură în cazurile suportate; refuz corect când este imposibil |
| F05 CRITICAL | EXECUTION_AND_RECONCILIATION: identități distincte și absență dovedită | Timeout acceptat, istoric întârziat, lipsa dublării |
| F06 CRITICAL | EXECUTION_AND_RECONCILIATION: tranziții și gărzi | Fill înainte de ACK, cancel/fill race, evenimente târzii |
| F07 CRITICAL | DATA_AND_LEDGER: balans per activ și outbox/inbox atomic | Taxe, reluare, corecții, crash și reconstrucție |
| F08 CRITICAL | DATA_AND_LEDGER: proprietate internă și suspense | Două moduri pe același cont, tranzacții externe și dust |
| F09 CRITICAL | RISK_SECURITY: equity, drawdown, limite și latches | Calcule, configurații reale, corelații/stres și resetare sigură |
| F10 HIGH | MARKET_DATA: disponibilitate temporală și calitate | Ingestie, goluri, duplicări, corecții, paritate între etape |
| F11 CRITICAL | TESTING_AND_CERTIFICATION: praguri și manifest de dovezi | Teste executate și observație reală, fără treceri din declarații |
| F12 CRITICAL | TESTING_AND_CERTIFICATION / DOMAIN_MODEL: certificat pe domeniu | Invalidare la schimbarea build/model/adapter/cont/config |
| F13 HIGH | AI_PROVIDER_ROUTING / CLAUDE_SUBSCRIPTION_SPIKE | Autentificare nativă și probă locală fără rută plătită implicită |
| F14 HIGH | AGENT_RUNTIME: task-uri, lease, bugete și evaluări | Skill-uri reale, restart, deduplicare și calitate pe cazuri reținute |
| F15 HIGH | AGENTS_AND_SKILLS / ADR-0001: sursa live deterministă | Agentul nu poate falsifica producătorul sau regimul live |
| F16 HIGH | STRATEGY_MODEL_LIFECYCLE: toate observațiile și trial-urile | Captură completă și separare între rezultat real și contrafactual |
| F17 HIGH | CODEX_TASK_GRAPH: securitate/cercetare/recuperare devreme | Aplicarea ordinii de dependență în implementare |
| F18 HIGH | ADR-0007: stack local minimal și componente condiționate | Benchmark și justificarea fiecărui serviciu suplimentar |
| F19 CRITICAL | LOCAL_DEPLOYMENT: fencing, restore, RPO/RTO | Exerciții pe host real, reactivare controlată și recuperare exchange |
| F20 HIGH | SECURITY / RISK_SECURITY: auth, CSRF, origine și izolare | Teste de acces/secret leakage/prompt injection și configurație OS |
| F21 MEDIUM | DOMAIN_MODEL: float statistic versus Decimal financiar | Conversii, NaN/Inf, precizie, tick/step și taxe |
| F22 HIGH | LOCAL_DEPLOYMENT / AGENT_RUNTIME: SLO, cozi și resurse | Măsurare sub sarcină, alerte și prioritate pentru protecție |
| F23 HIGH | ADR-0007 / OPEN_SOURCE_ADOPTION: un singur motor autoritativ | Spike Nautilus/adapter și imposibilitatea ocolirii riscului |
| F24 MEDIUM | PRODUCT_REQUIREMENTS: trei tipuri de acceptare | Raportarea onestă a unui motor sigur fără strategie eligibilă |

## 6. Ce rămâne de construit

Toate modulele executabile, schemele, migrațiile, testele, CI, interfața, depozitarea datelor, agenții și integrarea exchange sunt încă de implementat. Documentația reduce ambiguitatea acestei lucrări; nu o scurtează la „conectăm câteva API-uri”.

Rămân decizii tehnice bazate pe probe: versiunea și potrivirea motorului de execuție; toolchain-ul exact; setul inițial de tipuri de ordine; sursa și calitatea datelor; hardware și resurse; configurațiile de securitate/backup. Acestea pot fi rezolvate autonom în limitele contractelor, cu justificare și teste.

Rămân decizii ale proprietarului înainte de live: capitalul real, limitele de pierdere/expunere, politica de canary și creștere, contul și permisiunile exchange, destinația de backup, autentificarea Claude locală și activarea explicită L5/L6. Nu trebuie furnizate acum în cloud și nu au fost ghicite din dorința de a genera venit.

Rămân necunoscute științifice: existența unui avantaj după costuri, stabilitatea lui, suficiența eșantionului și comportamentul în regimuri viitoare. Niciun document, plugin sau agent nu poate elimina aceste incertitudini prin promisiune.

## 7. Ordinea următoare de lucru

| Prioritate | Lucrare și dependență | Rezultat verificabil |
|---|---|---|
| P0, T001–T006 | Pornire explicită a implementării; toolchain, scheme, prototip atomic, spike motor/Claude, formule/protocol | Bază executabilă și ipoteze critice rezolvate sau blocate explicit |
| P1, T007–T015 | După contractele P0: date cauzale, ledger, risc/OPA, execuție simulată, reconciliere, cercetare și UI minim | L1/L2 pe dovezi; rezultat economic separat |
| P2, T016–T023 | După L1/L2: paper/shadow, runtime real, skill-uri, Loki, UI complet, securitate/recovery și Claude local | L3/L4 observate și capabilități AI demonstrate |
| P3, T024–T026 | După eligibilitate, controale și acțiuni locale ale proprietarului | Canary real limitat, apoi L6 pe domeniu și activare explicită |
| P4, T027–T030 | După dovezile precedente: operare susținută, rampă controlată și verificare finală | Acceptarea completă Enterprise Local, fără a ascunde lipsurile unui mod |
| FUTURE | După justificarea economică a produsului global | SaaS separat, fără a încărca acum nucleul local |

Primul pas concret la lansarea implementării este un ExecPlan pentru T001. Nu este necesară repetarea întregului audit sau instalarea de framework-uri de agenți înainte de contracte și prototipuri. Cercetarea suplimentară trebuie să răspundă unei incertitudini concrete din spike-uri, nu să adauge tehnologii la întâmplare.

## 8. Biblioteci, plugin-uri și costuri

GitHub rămâne util pentru modificări verificabile, istoric și checkpoint-uri. Documentația oficială Binance/Claude trebuie verificată la integrare și upgrade. Hypothesis, fixture-uri independente și fault injection oferă valoare directă pentru bani și recuperare. Instrumentele statistice mature reduc codul numeric inutil, dar nu validează automat o strategie.

Nautilus se evaluează înainte de un motor propriu; PostgreSQL este baza tranzacțională; Parquet/DuckDB sunt suficiente ca primă ipoteză de analiză locală. OPA rămâne parte din autoritate. OpenBao și serverele de evenimente/analitice se justifică prin cerințe și măsurători. Nu am instalat plugin-uri, nu am activat servicii plătite și nu am importat componente cu obligații de licență neclarificate.

Un plugin de analiză de securitate poate fi util când există cod, după verificarea accesului și costului. Skill-uri proprii pentru verificarea invariantelor și protocolului de cercetare pot standardiza recenziile, dar trebuie legate de teste și date reale. Nu recomand un nou agent permanent doar pentru a avea un nume pentru fiecare verificare.

## 9. Ce nu trebuie făcut acum

Nu se construiesc Kubernetes, Kafka, service mesh, baze distribuite, multi-region, tenant billing, SSO complex, organizații sau suport global. Nu sunt necesare unsprezece procese LLM permanente, un vector database plătit pentru Loki ori conversații continue între agenți.

Nu se activează implicit API plătit când se termină abonamentul. Nu se extrag tokenuri din Claude. Nu se oferă LLM-ului un MCP cu ordine brute. Nu se dezvoltă simultan două motoare autoritative de execuție. Nu se folosesc reinforcement learning, martingale sau modificări live autonome pentru a compensa lipsa unui avantaj demonstrat.

Securitatea locală, autentificarea, izolarea cheilor, jurnalul, backup-ul și recuperarea nu se amână. Faza SaaS va adăuga securitate și operațiuni pentru mai mulți clienți, peste o bază locală deja sigură și testată.

## 10. Verificări ale acestei livrări

Verificarea documentară controlează inventarul, păstrarea fișierelor originale, legăturile relative, referințele canonice, cei unsprezece agenți, dependențele T000–T030 și corespondența F01–F24. Comenzile și rezultatele sunt în DOCUMENTATION_CHECKS_2026-10-04 și VALIDATION_LOG. Publicarea este un commit de documentație, fără modificarea vizibilității, licenței sau configurației repository-ului.

Recenzia adversarială a urmărit separat contradicțiile: eveniment versus comandă; retry versus nouă intenție; agent de regim versus clasificator live; cost dependent de cantitate versus dimensionare; DB indisponibil versus autoritate de urgență; „permanent” versus apel continuu; L6 versus profit; documentație finalizată versus produs implementat. Corecțiile sunt în specificațiile lor autoritative.

Aceste verificări nu sunt teste ale aplicației. Nicio bifă din raport nu certifică un ordin, un agent sau un model care încă nu există în cod. Raportul Word de livrare include identificatorul commit-ului publicat și rezultatul verificării arborelui remote.

## 11. Inventarul schimbărilor

Căile sunt relative la repository. „Modificat” înseamnă revizuire documentară; „păstrat identic” înseamnă conținut identic cu snapshot-ul inițial. Nicio cale originală nu a fost eliminată.

| Fișier | Stare | Rol / modificare |
|---|---|---|
| `.agent/PLANS.md` | Păstrat identic | Standardul ExecPlan păstrat. |
| `AGENTS.md` | Modificat | Invariante și autoritate aliniate; separarea documentației de implementare. |
| `CONTRIBUTING.md` | Modificat | Dovezi documentare separate de probe executabile. |
| `README.md` | Modificat | Țintă, stare L0 și traseu de lectură actualizate. |
| `SECURITY.md` | Modificat | Securitate locală, origine browser, izolare și chei. |
| `docs/adr/ADR-0001-financial-trust-boundary.md` | Modificat | Decizie arhitecturală; clarificare de autoritate. |
| `docs/adr/ADR-0002-agent-identity-independent-of-model.md` | Păstrat identic | Decizie arhitecturală originală păstrată. |
| `docs/adr/ADR-0003-risk-direction-and-emergency-reduction.md` | Modificat | Decizie arhitecturală; clarificare de autoritate. |
| `docs/adr/ADR-0004-cloud-development-never-holds-live-secrets.md` | Păstrat identic | Decizie arhitecturală originală păstrată. |
| `docs/adr/ADR-0005-math-mode-quantitative-authority.md` | Păstrat identic | Decizie arhitecturală originală păstrată. |
| `docs/adr/ADR-0006-l5-bounded-canary-before-l6.md` | Păstrat identic | Decizie arhitecturală originală păstrată. |
| `docs/adr/ADR-0007-local-modular-deployment.md` | Adăugat | Decizie arhitecturală nouă, implementare încă necesară. |
| `docs/adr/ADR-0008-event-driven-agents-and-subscription-runtime.md` | Adăugat | Decizie arhitecturală nouă, implementare încă necesară. |
| `docs/adr/ADR-0009-atomic-financial-authority-and-scoped-evidence.md` | Adăugat | Decizie arhitecturală nouă, implementare încă necesară. |
| `docs/architecture/ARCHITECTURE_V1.md` | Modificat | Patru planuri și nucleu local modular. |
| `docs/architecture/OPEN_SOURCE_ADOPTION.md` | Modificat | Reutilizare motivată, licențe și condiții. |
| `docs/audit/AUDIT_2026-10-02.md` | Păstrat identic | Proveniență istorică păstrată identic. |
| `docs/audit/DOCUMENTATION_CHECKS_2026-10-04.md` | Adăugat | Checker reproductibil și limitele verificării. |
| `docs/audit/DOCUMENTATION_REMEDIATION_2026-10-04.md` | Adăugat | Raportul de comparație și lucrările rămase. |
| `docs/audit/INDEPENDENT_AUDIT_2026-10-04.md` | Adăugat | Audit independent complet, snapshot original. |
| `docs/plans/2026-10-04-documentation-remediation.md` | Adăugat | Plan de lucru, verificare și checkpoint al acestei livrări. |
| `docs/program/DECISIONS.md` | Modificat | Istoric păstrat, deciziile noi adăugate. |
| `docs/program/KNOWN_ISSUES.md` | Modificat | Lipsuri și dovezi încă necesare. |
| `docs/program/STATUS.md` | Modificat | Checkpoint onest L0 și următoarea acțiune. |
| `docs/program/VALIDATION_LOG.md` | Modificat | Istoric păstrat, verificarea documentară adăugată. |
| `docs/roadmap/AUTONOMOUS_ENTERPRISE_LOCAL_GOAL.md` | Modificat | Rezultat complet și blocaje raportate corect. |
| `docs/roadmap/CODEX_MASTER_PROMPT.md` | Modificat | Implementare autonomă fără compromisuri ascunse. |
| `docs/roadmap/CODEX_RESUME_PROTOCOL.md` | Păstrat identic | Limitele și reluarea platformei păstrate. |
| `docs/roadmap/CODEX_TASK_GRAPH.md` | Modificat | T000–T030 cu dependențe și criterii. |
| `docs/roadmap/IMPLEMENTATION_ROADMAP.md` | Modificat | P0–P4 și FUTURE; controale timpurii. |
| `docs/roadmap/PHASE_0_BOOTSTRAP.md` | Modificat | Primele contracte, prototipuri și probe. |
| `docs/specs/AGENTS_AND_SKILLS.md` | Modificat | Unsprezece responsabilități și skill-uri reale. |
| `docs/specs/AGENT_RUNTIME.md` | Adăugat | Task-uri persistente, cozi, cotă și recuperare. |
| `docs/specs/AI_PROVIDER_ROUTING.md` | Modificat | Rutare Claude, cost și izolare, fără proxy. |
| `docs/specs/CAPITAL_GROWTH.md` | Modificat | Alocare fixă inițială și creștere controlată. |
| `docs/specs/CLAUDE_SUBSCRIPTION_SPIKE.md` | Adăugat | Proba locală obligatorie a rutei de abonament. |
| `docs/specs/DATA_AND_LEDGER.md` | Modificat | Balans per activ, atribuire și tranzacții atomice. |
| `docs/specs/DOMAIN_MODEL.md` | Modificat | Entități financiare, agent și certificare extinse. |
| `docs/specs/EVENT_CATALOG.md` | Modificat | Evenimente, comenzi, versiuni și replay sigur. |
| `docs/specs/EXECUTION_AND_RECONCILIATION.md` | Modificat | Ordine, tranziții, ambiguitate și recuperare. |
| `docs/specs/FINANCIAL_AUTHORIZATION.md` | Adăugat | Atomicitate, rezervări, identități și fencing. |
| `docs/specs/LOCAL_DEPLOYMENT.md` | Modificat | Deploy local, restore, backup și operare. |
| `docs/specs/LOKI.md` | Adăugat | Ghid permanent, surse, autoritate și evaluări. |
| `docs/specs/MARKET_DATA.md` | Adăugat | Date disponibile la momentul deciziei și calitate. |
| `docs/specs/PRODUCT_REQUIREMENTS.md` | Adăugat | Acceptarea produsului complet și limita SaaS. |
| `docs/specs/QUANTITATIVE_METHODS.md` | Adăugat | Candidați, formule, execuție și protocol științific. |
| `docs/specs/RISK_SECURITY.md` | Modificat | Formule, limite, latches și matrice de urgență. |
| `docs/specs/SERVICE_CONTRACTS.md` | Modificat | Proprietari logici și tranzacții financiare. |
| `docs/specs/STRATEGY_MODEL_LIFECYCLE.md` | Modificat | Toate observațiile, respingere și promovare. |
| `docs/specs/TESTING_AND_CERTIFICATION.md` | Modificat | Porți măsurabile și certificare pe domeniu. |
| `docs/specs/TRADE_INTENT.md` | Modificat | Propunere versus autoritate; unități și surse. |
| `docs/specs/TRADING_MODES.md` | Modificat | Fluxurile celor două moduri și contul comun. |
| `docs/specs/UI_COMMAND_CENTER.md` | Modificat | Fluxuri utilizabile, Loki și stări oneste. |
| `start.md` | Modificat | Pornire explicită și aceleași cerințe pentru implementator. |

## 12. Verdictul final

Documentația este acum o bază mult mai precisă pentru implementarea Enterprise Local cerută. Îmbunătățirea principală este că viitorul implementator are limite, stări, responsabilități și porți verificabile, iar produsul nu depinde conceptual de o fază SaaS sau de apeluri AI continue.

Ce rămâne neschimbat: cele două moduri, agenții ca responsabilități permanente, granița financiară deterministă, controlul proprietarului și ținta locală completă.

Ce trebuie demonstrat înainte de bani reali: implementarea corectă, validarea candidaților după costuri, integrarea Claude locală, controalele de execuție/risc/contabilitate, recuperarea și dovezile L1–L6. Profitabilitatea rămâne o întrebare empirică.

Ce așteaptă SaaS: operarea comercială pentru mai mulți clienți, tenant isolation, identity/billing/compliance și scalarea globală justificată economic.

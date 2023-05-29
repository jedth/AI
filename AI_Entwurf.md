# Einführung und Ziele

## Aufgabenstellung

## Qualitätsziele

## Stakeholder

| Rolle                      | Erwartungshaltung                                                                     |
| -------------------------- | ------------------------------------------------------------------------------------- |
| **Kunden**                 | einfach und unkompliziert sein Gerät minimale benötigte Daten angeben für den Verkauf |
| **Mitarbeiter im Betrieb** | muss koordiniert und reibungslos ablaufen                                             |

# Randbedingungen

# Kontextabgrenzung

![Kontextsicht](images/Kontextsicht_AI.png)

## Fachlicher Kontext

**\<Diagramm und/oder Tabelle>**

**\<optional: Erläuterung der externen fachlichen Schnittstellen>**

## Technischer Kontext

**\<Diagramm oder Tabelle>**

**\<optional: Erläuterung der externen technischen Schnittstellen>**

**\<Mapping fachliche auf technische Schnittstellen>**

# Lösungsstrategie

# Bausteinsicht

## Ebene 1
![Bausteinsicht Ebene](images/bausteinsicht_ebene1.png)

## Ebene 2
![Bausteinsicht Ebene 2](images/bausteinsicht_ebene2.png)

### ReviewService

<table>
    <thead>
        <tr>
            <th>Kriterium</th>
            <th>Beschreibung</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Zweck/Verantwortung</td>
            <td>Preisschätzung:<br>Durchführung einer Preisschätzung mit Angaben des Kunden<br>Vollständige Bewertung:<br>Durchführung der Bewertung mit den angegebenen Bewertungsmerkmalen (Preisberechnung)</td>
        </tr>
        <tr>
            <td>Schnittstellen</td>
            <td>GeräteKatalog: stellt die für die Bewertung benötigten Regeln bereit (getRules)<br>ProduktManager: kann die Erstellung von Bewertungen anfragen (createReview)</td>
        </tr>
        <tr>
            <td>Qualitäts-/ Leistungsmerkmale
            </td>
            <td>Genauigkeit: Preisschätzung soll möglichst akkurat sein (nah an eigentlicher Bewertung)<br>Korrektheit: berechneter Preis entspricht den Bewertungskriterien</td>
        </tr>
        <tr>
            <td>Offene Probleme/Risiken</td>
            <td>Mitarbeiter können bei der Bewertung auf noch unbekannte technische Probleme stoßen (für diese gibt es noch keine Fragen im Fragenkatalog)</td>
        </tr>
    </tbody>
</table>

#### Schnittstellen

<table>
    <thead>
        <tr>
            <th>Schnittstelle</th>
            <th>Vorbedingung</th>
            <th>Nachbedingung</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>createReview(<br>&emsp;DeviceType,<br>&emsp;List&lt;Properties&gt;<br>)</code></td>
            <td>Gerätetyp wird unterstützt, Produkteigenschaften müssen vorhanden sein</td>
            <td>eine korrekte Bewertung des angegebenen Geräts wurde zurückgegeben</td>
        </tr>
        <tr>
            <td><code>getRules(DeviceType)</code></td>
            <td>Gerätetyp wird unterstützt</td>
            <td>Regeln werden zurückgegeben</td>
        </tr>
    </tbody>
</table>

### DeviceCatalog

<table>
    <thead>
        <tr>
            <th>Kriterium</th>
            <th>Beschreibung</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Zweck/Verantwortung</td>
            <td>
                <ul>
                    <li>Verwaltung der unterstützten Gerätetypen<br></li>
                    <li>nimmt 5-10 neue Gerätetypen pro Monat auf<br></li>
                    <li>sollte es zu einem bereits unterstützten Gerätetypen ein Jahr lang keine Einsendungen geben, wird dieser aus dem Angebot entfernt<br></li>
                    <li>speichert Informationen zu den Gerätetypen:<br></li>
                    <li>Bewertungskriterien<br></li>
                    <li>Preis<br></li>
                    <li>mögliche verwertbare Subkomponenten/Teile<br></li>
                    <li>Anzahl eingesendeter Geräte<br></li>
                    <li>Zeitpunkt der Einsendungen</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Schnittstellen</td>
            <td>
                <li>BewertungsService: greift auf die gespeicherten Regeln zu einem Gerätetypen zu (getRules)<br></li>
                <li>ProduktManager: kann anfragen, ob ein Gerät unterstützt wird (isSupportedType)<br></li>
                <li>ProduktManager: kann Unterstützung eines neuen Gerätetyps anfragen (addSupportedType)<br></li>
                <li>GeräteDatenbank: stellt Daten zu den Gerätetypen bereit (z.B. getNumberOfDevices)<br></li>
                <li>ERP-System: wird über Änderungen bezüglich der Unterstützung von Gerätetypen und erhaltenen Geräten informiert (informERP)</td>
            </li>
        </tr>
        <tr>
            <td>Qualitäts-/ Leistungsmerkmale</td>
            <td>
                <li>Korrektheit: die gespeicherten Information müssen korrekt sein<br></li>
                <li>Aktualität: die gespeicherten Informationen dürfen nicht veraltet sein</li>
            </td>
        </tr>
        <tr>
            <td>Offene Probleme/Risiken</td>
            <td>
                <li>mögliche Duplizierung von Daten, die schon im ERP-System gespeichert werden</li>
            </td>
        </tr>
    </tbody>
</table>

#### Schnittstellen

<table>
    <thead>
        <tr>
            <th>Schnittstelle</th>
            <th>Vorbedingung</th>
            <th>Nachbedingung</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>getRules(DeviceType)</code></td>
            <td>Gerätetyp wird unterstützt</td>
            <td>Regeln werden zurückgegeben</td>
        </tr>
        <tr>
            <td><code>isSupportedType(DeviceType)</code></td>
            <td>Gerät wurde eingesendet</td>
            <td>Rückgabe, ob der angegebene Gerätetyp unterstützt wird</td>
        </tr>
        <tr>
            <td><code>addSupportedType(DeviceType)</code></td>
            <td>Gerätetyp wird (noch) nicht unterstützt</td>
            <td>Gerätetyp wird unterstützt</td>
        </tr>
        <tr>
            <td><code>getNumberOfDevices(DeviceType)</code></td>
            <td>Gerätetyp wird unterstützt und ist in GeräteDatenbank gespeichert</td>
            <td>Anzahl der Geräte wird an GeräteKatalog zurückgegeben</td>
        </tr>
        <tr>
            <td><code>informERP(List&lt;Device&gt;)</code></td>
            <td>Daten sind mit ERP-System kompatibel</td>
            <td>ERP-System ist über die neuen Informationen informiert</td>
        </tr>
    </tbody>
</table>

# Laufzeitsicht

## *\<Bezeichnung Laufzeitszenario 1>*

-   ![Laufzeitsicht](images/laufzeitsicht.png)

-   \<hier Besonderheiten bei dem Zusammenspiel der Bausteine in diesem
    Szenario erläutern>

# Verteilungssicht

## Infrastruktur Ebene 1

![Verteilungssicht](images/verteilungssicht.png)

Begründung  
*\<Erläuternder Text>*

Qualitäts- und/oder Leistungsmerkmale  
*\<Erläuternder Text>*

Zuordnung von Bausteinen zu Infrastruktur  
*\<Beschreibung der Zuordnung>*

## Infrastruktur Ebene 2

### *\<Infrastrukturelement 1>*

*\<Diagramm + Erläuterungen>*

### *\<Infrastrukturelement 2>*

*\<Diagramm + Erläuterungen>*

…

### *\<Infrastrukturelement n>*

*\<Diagramm + Erläuterungen>*

# Querschnittliche Konzepte

## *\<Konzept 1>*

*\<Erklärung>*

## *\<Konzept 2>*

*\<Erklärung>*

…

## *\<Konzept n>*

*\<Erklärung>*

# Architekturentscheidungen

# Qualitätsanforderungen


**Datenschutz und Datenintegrität:**
- Persönliche Daten der Kunden sind geschützt. Kunden können nur auf ihre eigenen Informationen zugreifen. Mitarbeiter sehen nur die relevanten Informationen zur Verarbeitung des Workflows. 

**Bedienbarkeit:**
- Kunden sollten sich einfach auf der Website orientieren können, System soll sehr handlich gestaltet sein. 

**Verlässlichkeit:**
- Das System speichet die von Kunden eingegebenen Daten korrekt. 

**Skalierbarkeit:**
- Das System kann sowohl mit 100 oder auch Millionen von Kunden umgehen. 

<div class="formalpara-title">

**Weiterführende Informationen**

</div>

Siehe [Qualitätsanforderungen](https://docs.arc42.org/section-10/) in
der online-Dokumentation (auf Englisch!).

## Qualitätsbaum

## Qualitätsszenarien

# Risiken und technische Schulden

# Glossar

| Begriff        | Definition        |
| -------------- | ----------------- |
| *\<Begriff-1>* | *\<Definition-1>* |
| *\<Begriff-2>* | *\<Definition-2>* |

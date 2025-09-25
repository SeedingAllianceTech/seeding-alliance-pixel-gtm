# Seeding Alliance Pixel GTM Template

Dieses Dokument beschreibt die Konfiguration und Funktionsweise des "Seeding Alliance Pixel" Tag-Templates für den Google Tag Manager. Das Template ermöglicht es Nutzern, Page Views und Conversions zu erfassen, die durch Seeding Alliance-Kampagnen generiert wurden.
---

## Funktionsweise

Das Tag-Template identifiziert Nutzer, die über eine Seeding Alliance-Kampagne auf Ihre Website gelangen, indem es nach dem URL-Parameter `nat_clid` (Click-ID) sucht.

1.  **Erfassung der Click-ID**: Wenn ein Nutzer auf Ihrer Seite landet, sucht das Skript in der aktuellen URL (oder der Referrer-URL) nach dem `nat_clid`-Parameter.
2.  **Cookie-Speicherung**: Die gefundene Click-ID wird in zwei 1st Party Cookies gespeichert: `nativendo_clid` und `nativendo_consent`. Dies stellt sicher, dass die Zuordnung auch bei späteren Aktionen des Nutzers (z.B. einer Conversion) erhalten bleibt. Die Cookies haben eine Lebensdauer von 30 Tagen (`2592000` Sekunden).
3.  **Pixel-Auslösung**: Basierend auf der Konfiguration des Tags wird ein Tracking-Pixel an die Seeding Alliance-Server gesendet, um entweder einen Seitenaufruf (`page_view`) oder eine Conversion zu melden.

---

## Einrichtung des Tags

**Bevor Sie das Tag konfigurieren können, müssen Sie die Vorlagendatei (`.tpl`) in Ihren Google Tag Manager-Container importieren.**

### **Vorlage importieren (Einmaliger Schritt)**

1.  **Navigieren Sie zu den Vorlagen**: Klicken Sie in der linken Navigationsleiste Ihres GTM-Containers auf **"Vorlagen"**.
2.  **Neue Tag-Vorlage erstellen**: Suchen Sie die Box mit der Überschrift **"Tag-Vorlagen"** und klicken Sie auf den Button **"Neu"**.
3.  **Importieren**: Klicken Sie im Vorlagen-Editor oben rechts auf das Dreipunkt-Menü (Weitere Aktionen) und wählen Sie **"Importieren"**.
4.  **Datei auswählen**: Wählen Sie die `template.tpl`-Datei aus, die Sie erhalten haben, und laden Sie diese hoch.
5.  **Speichern**: Nach dem erfolgreichen Import klicken Sie auf **"Speichern"** und schließen den Vorlagen-Editor.

Die Vorlage "Seeding Alliance Pixel" ist nun in Ihrem Container verfügbar und kann beim Erstellen eines neuen Tags ausgewählt werden.

---

Um das Tag zu konfigurieren, füllen Sie die folgenden Felder in der Google Tag Manager-Oberfläche aus.

### Tag-Konfiguration

#### Event
Dies ist das Hauptfeld, das den Zweck des Tags bestimmt.

* **Page View**: Wählen Sie diese Option, um einen allgemeinen Seitenaufruf zu erfassen. Dieses Tag sollte idealerweise auf allen Seiten ausgelöst werden, auf denen der Kampagnen-Traffic landen kann. Es sendet ein `page-view`- und ein `page-consent`-Signal.
* **Conversion**: Wählen Sie diese Option, um eine bestimmte Nutzeraktion zu erfassen, z. B. einen Kauf, eine Anmeldung oder einen Lead. Wenn diese Option ausgewählt ist, werden zusätzliche Konfigurationsfelder sichtbar.

---

### Conversion-Einstellungen
Diese Felder sind **nur sichtbar**, wenn als **Event** der Wert `Conversion` ausgewählt wurde.

#### Typ
Wählen Sie die Art der Conversion, die Sie erfassen möchten.

* **Vordefinierte Typen**: Es steht eine Liste von Standard-Conversion-Typen zur Verfügung (z. B. `Purchase`, `Lead`, `Signed up`, `Add to cart`). Wählen Sie den Typ, der am besten zu Ihrer Aktion passt.
* **Custom**: Wenn keiner der vordefinierten Typen passt, wählen Sie `Custom`. Daraufhin erscheint ein zusätzliches Feld, in dem Sie einen eigenen Conversion-Typ definieren können.

#### Custom Type (benutzerdefinierter Typ)
Dieses Feld erscheint nur, wenn als **Typ** `Custom` ausgewählt wurde.
* Geben Sie hier einen Namen für Ihren benutzerdefinierten Conversion-Typ ein.
* **Formatvorgabe**: Der Name muss mit einem Kleinbuchstaben beginnen und darf nur Kleinbuchstaben, Zahlen und Unterstriche (`_`) enthalten. Beispiel: `special_promo_signup`.

#### Order Value (Bestellwert)
Aktivieren Sie diese Checkbox, um einen monetären Wert mit der Conversion zu übermitteln. Dies ist besonders für `Purchase`-Events relevant.

* **Value (Wert)**: Geben Sie den numerischen Wert der Conversion an (z. B. den Warenkorbwert). Es können bis zu 8 Nachkommastellen verwendet werden. Beispiel: `99.95`.
* **Currency (Währung)**: Die Währung ist standardmäßig auf `EUR` festgelegt.

#### Custom Parameters (benutzerdefinierte Parameter)
Hier können Sie zusätzliche Daten als Schlüssel-Wert-Paare an die Conversion anhängen.

* **Parameter Name**: Der Name des Parameters.
    * **Wichtig**: Die Namen `type`, `order_value` und `order_currency` sind reserviert und können hier nicht verwendet werden.
    * **Formatvorgabe**: Muss mit einem Kleinbuchstaben beginnen und darf nur Kleinbuchstaben, Zahlen und Unterstriche (`_`) enthalten.
* **Parameter Value**: Der Wert, der für den Parameter übergeben werden soll.

**Beispiel für benutzerdefinierte Parameter:**

| Parameter Name | Parameter Value |
| --- | --- |
| `product_id` | `{{DLV - Product ID}}` |
| `payment_method` | `credit_card` |
| `customer_status` | `new` |

---

## Anwendungsbeispiele

### Beispiel 1: Page View Tracking

Dieses Tag soll auf allen Seiten ausgelöst werden, um den Traffic der Kampagne zu erfassen.

* **Event**: `Page View`
* **Trigger**: `All Pages`



### Beispiel 2: Kauf-Conversion mit dynamischem Wert

Dieses Tag soll auf der "Dankeschön"-Seite nach einem Kauf ausgelöst werden.

* **Event**: `Conversion`
* **Type**: `Purchase`
* **Order Value**: ☑️ Aktiviert
    * **Value**: `{{DLV - Transaction Total}}` *(eine GTM-Variable, die den Bestellwert enthält)*
    * **Currency**: `EUR`
* **Custom Parameters**:
    * Parameter Name: `transaction_id`, Parameter Value: `{{DLV - Transaction ID}}`
* **Trigger**: Ein benutzerdefiniertes Ereignis `purchase`, das vom Data Layer ausgelöst wird.
# Για χρήστες του Eversense

The easiest way to use Eversense with AAPS is to install the EU or US modified [Eversense app](https://cr4ck3d3v3r53n53.club/) (and uninstall the original one first).

**Warning: by uninstalling the old app, your local historical data older than one week will be lost!**

To finally get your data to AAPS, you need to install [ESEL](https://github.com/BernhardRo/Esel/releases) and enable "Send to AAPS and xDrip" in ESEL and "MM640g" as BG source in the [Configuration Builder](../Configuration/Config-Builder.md) in AAPS. Δεδομένου ότι τα δεδομένα BG από το Eversense μπορεί να είναι θορυβώδη μερικές φορές, καλό είναι να ενεργοποιείτε τα "Smooth Data" στο ESEL, κάτι που είναι καλύτερο από το να επιτρέπετε "Να χρησιμοποιείτε πάντα το σύντομο μέσο delta αντί για το απλό delta" στο AAPS.

You can find the instruction for using xDrip with an Eversense [here](https://github.com/BernhardRo/Esel/tree/master/apk).

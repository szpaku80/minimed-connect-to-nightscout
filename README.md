# minimed-connect-to-nightscout

[MiniMed Connect] is a device which communicates with a Medtronic insulin pump over 916MHz radio to gather data about pump status. This data may include readings from an Enlite continuous glucose monitor (CGM). The Connect forwards pump data via Bluetooth LE to a [proprietary app][connect-app] running on an iPhone. The iPhone app uploads the data to Medtronic's servers, enabling authorized users to view it on the [Medtronic CareLink Connect][carelink] website.

[Nightscout] is an open source, DIY project that allows real-time access to CGM data on any platform. The core of the Nightscout project is [cgm-remote-monitor], a web service which can receive and broadcast CGM data using a Mongo database. Various other software projects communicate with an instance of `cgm-remote-monitor`, either to transmit data from a CGM device, or to display that CGM data on a computer, smartphone/tablet, or smartwatch.

**minimed-connect-to-nightscout** is a web scraper which sends data from Medtronic CareLink Connect to Nightscout. It does so by posing as a web browser, logging into CareLink Connect, periodically downloading pump status data from the Medtronic server, and uploading that data to a Nightscout server.

## Prerequisites

* A [MiniMed Connect] and compatible Medtronic insulin pump
* An iPhone running the [MiniMed Connect app][connect-app]
* Username and password for a [CareLink][carelink] account linked to the Connect
* A working Nightscout website and Mongo database

## Install

1. Install [PhantomJS].
1. Clone this repository or [download a zip] with the latest version.
1. Create a `config.js` file based on the provided `config.js.example`.
1. Run `phantomjs scraper.js` and leave it running.

**Coming soon:** Deploy instructions for Heroku

## Currently supported data

* Sensor glucose values
* Active insulin (sent to Nightscout as entries of type `reported_active_insulin`)
* Pump model and serial number (included in all Nightscout entries)

**Potential future data**: See [this gist] for sample data provided by CareLink Connect. In particular, I'd love to include BG trend (up/down arrows) based on `lastSGTrend`. Please [get in touch] if you use an Enlite sensor and would like to help.

## Notes

Besides PhantomJS, there are dependencies on [CasperJS][casper] (for nicer interaction with PhantomJS) and [Rusha][rusha] (for crypto to authenticate with Nightscout). Since this is neither a Node module nor runnable with Node, they are included in the repo under `vendor` instead of adding a dependency on npm.

[File an issue] if you'd like to give feedback, request an enhancement, or report a bug. Pull requests are welcome.

## Disclaimer

This project is intended for educational and informational purposes only. It relies on a series of fragile components and assumptions, any of which may break at any time. It is not FDA approved and should not be used to make medical decisions. It is neither affiliated with nor endorsed by Medtronic, and may violate their Terms of Service agreement.

[MiniMed Connect]: http://www.medtronicdiabetes.com/products/minimed-connect
[connect-app]: https://itunes.apple.com/us/app/minimed-connect/id999836914
[carelink]: https://carelink.minimed.com/
[Nightscout]: http://www.nightscout.info/
[cgm-remote-monitor]: https://github.com/nightscout/cgm-remote-monitor
[PhantomJS]: http://phantomjs.org/
[download a zip]: https://github.com/mddub/minimed-connect-to-nightscout/archive/master.zip
[this gist]: https://gist.github.com/mddub/b033ec0c800deec02471
[get in touch]: mailto:mark@warkmilson.com
[File an issue]: https://github.com/mddub/minimed-connect-to-nightscout/issues
[casper]: http://casperjs.org/
[rusha]: https://www.npmjs.com/package/rusha
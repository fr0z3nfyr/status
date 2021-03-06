> **App Status Planel CakePHP Plugin**
> 
> Copyright (c) 2009 Matt Curry
> 
> www.PseudoCoder.com
> 
> http://github.com/fr0z3nfyr/status (Forked from: http://github.com/mcurry/status)
> 
> @author      Matt Curry <matt@pseudocoder.com>
> 
> @license     MIT

 
### About
  One convienent page where you can get info about your app.  Google analytics, log files, cron'd shells, etc.  In addition to the core panels it is simple to add your own.

### Instructions
1. Download the plugin to `/app/plugins/status`
2. Add the panels you want active in your `bootstrap.php`:
	```
	Configure::write('Status.panels', array(
		'Status.google_analytics' => array('visits', 'referrers', 'keywords'),
		'Status.system',
		'Status.shell',
		'Status.tests'
		'Status.logs' => array('error', 'debug'),
	));
	```
3. By default no one can reach the status panel.  It is up to you to deteremine who will have access.  In your AppController::beforeFilter() put:
	```
	if(...) {
		Configure::write('Status.allow', true);
	}
	```
4. Go to `http://yourapp/status`
	
### Core Panels
#### Google Analytics
Multiple panels for quick info on your visits, refererrs and search engine keywords using the Google Analytics API.  Adjustable timeframes to get a recent or long range snapshot.
You must set up the config file to access your analytics via Google's API:
1. Rename `/app/plugins/status/config/status.php.default` to `status.php`
2. Edit the file to include your Google email and password.
	 The account_id can be found in the url at Google (https://www.google.com/analytics/reporting/?reset=1&id=xxxxxxxx)
	
#### Shell 
Add logging to your shells to capture results.  Great for shells that run from cron.  Clicking on the shell link in the panel pops open a window with detailed information.  Requires integrating the log task in your shell:
1. Run the SQL in /app/plugins/status/config/sql/status_consoles.sql
2. Include the task in your shell's attributes:
   ```
   var $tasks = array('Log');
   ```
3. Start the logging in the beginning of your shell:
   ```
   $this->Log->start();
   ```
4. Store any messages you want to be available in the panel:
   ```
   $this->Log->out("Processed $count records");
   ```
5. Signal the script is done:
   ```
   $this->Log->end();
   ```

#### Tests
  Run your app's unit tests right from the panel, regardless of debug mode.  If there are any errors you can click on the summary line to get the full details.
	
#### System
  Basic infomation about the system hardware, including disk space and uptime.
  
#### Logs
  Show Cake's log files.  Can be used on any log file in /app/tmp/logs, including custom ones.  Just pass the log file name(s).

### Other Panels
#### Click Tracking
Track links that are click on your app.  Mainly used for outgoing affiliate links, but can also be used for downloaded files.
Refer ~~http://github.com/mcurry/click~~ https://github.com/fr0z3nfyr/click

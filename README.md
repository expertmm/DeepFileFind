# DeepFileFind
The goal of this project is to create the most satisfying search program in the world. DeepFileFind...

* Has features you need
* Isn't annoying
* Is GPL (free as in freedom, forever--redistribute modify according to the terms as you wish)

--in other words, it's a program like nothing before it.

## Key features
* Search partial name (without requiring any type of wildcard notation)
* Modification Date Range (time is optional for each)
* Open a found file by right-clicking on a result file (or optionally open the folder that contains it)
* Saves all possible settings to the user's profile (such as %APPDATA%\DeepFileFind\)

## Directives
* Intuitive: just try to use it and it works
* For average users (has justifiable and readily usable defaults; no regex (except perhaps an invisible option via settings file in the future)--it is unnecessary, and showing an option to use "regex" or "regular expressions" only serves to confuse most people)
* Isn't annoying (no search index; will never have panels other than search and results; no tabs)
* Skipping or modifying any of these directives will make it like any other search tool and therefore irrelevant.
* Cross-platform

## Changes
(! = important)
* (2017-12-01) (!) (treat blank search filename as "*") fix silent failure (no results) on blank filename, such as when doing search by only criteria other than name
* (2017-12-01) remove (wrong) version number from about box
* (2017-10-09) (!) removed use of HasAttrib for FileAttributes objects in order to compile on .NET 3.5 for Windows
* (2017-10-09) (!) Changes made today allow non-priveleged user to do content search of all files on '/' without crash (except special files and folders code now manually skips to work around issues; tested only with: Antergos, file size maximum set at 2048000 bytes)
* (2017-10-09) (!) skips "non-file-like" names to avoid native call crashes:
	* "*:"
	* "Singleton*"
	* "*Socket"
* (2017-10-09) (!) no longers follows the following types of folders (no longer recurses, by design)
	* Offline
	* Device
	* and the following are hard-coded booleans set to false:
		* dff.search_inside_hidden_files_enable
		* dff.follow_system_folders_enable

* (2017-10-09) commented out deprecated debugging output for "file: " date and "endbefore: " date comparison
* (2017-10-09) now has try block in get_is_match (but still has native crash as proven by using Console.WriteLine at beginning and end of it and end output not showing before or after error):
```
...
Got a SIGSEGV while executing native code. This usually indicates
a fatal error in the mono runtime or one of the native libraries 
used by your application.
...
```
* (2017-10-09) output recursion depth and folder name to stdout (0 means searching a user-specified Location)
* (2017-10-09) IF Path.DirectorySeparatorChar=='/', now a DFF object never recurses non-data paths (unless user puts /dev in Location TextBox, which would be silly, but why prevent people from being silly as long as they meant it) as per DFF's constructor, such as:
  ```
  if (Path.DirectorySeparatorChar=='/') non_data_paths = new string [] { "/dev", "/proc", "/sys", "/boot", "/run/udev"};
  ```
* (2017-10-09) MonoDevelop project Release configuration now outputs directly to <project path>/bin (instead of <project path>/bin/Release)
* (2017-10-09) (!) prevent infinite folder recursion in case of recursive symlinks, by not following symlinks: (avoid directories with ReparsePoint attribute; this method works on both Windows and Linux according to brad on https://stackoverflow.com/questions/5775739/prevent-io-getdirectories-from-following-symlinks answered Apr 25 '11 at 14:10)
	* new variable follow_folder_symlinks_enable is hardcoded as false
* (2017-06-06) Made MonoDevelop project (had to add assembly references that were in the old project file; had error loading icon resource; had to change Build Action of MainForm.resx to EmbeddedResource [error persisted after that]; had to set Text File profile to Visual Studio in order for XML resx file to load properly; compiles and runs now)
* (2017-06-01) Date should be saved and loaded (variable name in yml was mismatched for both start and end modification dates). Corrected loading by using ToLocalTime() method.
* (2017-06-01) Add wildcard notation
* (2017-05-30) Corrected tab order, taking into consideration tab order is hierarchical (sorted containers and non-contained widgets consecutively)
* (2017-05-30) Add sorting by any column
* (2017-05-16) Shows error on using wildcards (any invalid characters actually) since not implemented yet
* Implement content search
* Add created date range
* (2017-05-05) Added min/max sizes feature, and nonsense input checking for that and for modification dates
* (2017-04-15) Press enter to search (if name or content box has focus)
* (2017-04-15) Ctrl+C can copy path of selected result
* (2017-04-15) Re-enable buttons correctly after nonexistent folder was used
* (2017-04-15) save location at top of recent list; use most recent location on startup; only set a generated value for location on startup if no locations were found
* (2017-04-15) Save content string and name to settings

## Known Issues
* add interface to change the following settings (as of 2017-10-09 no longer searches for content in them, by design)
	* dff.search_inside_hidden_files_enable
	* dff.follow_system_folders_enable
* add interface to change the following settings (as of 2017-10-09 no longer recurses, by design)
	* dff.follow_folder_symlinks_enable
	* dff.follow_temporary_folders_enable
* add interface to change the following settings (as of 2017-10-09 booleans exist, but due to their starting values, the behavior of the program is the same as before in these ways):
	* dff.follow_dot_folders_enable //on by default (folders starting with '.')
	* dff.follow_hidden_folders_enable //on by default
	
* UI is nonresponsive during search except between files
* Finds no folders if "Include folders..." is checked but criteria is "*" or "*.*" (or empty).
* Make all savable variables always use dictionary, otherwise MainFormFormClosing and MainFormLoad must have the same list of variables, otherwise the mismatched settings aren't saved & loaded correctly
* Sort again at end of search
* Fix flicker on list when each file is found
* Warn&confirm if results would be every file in folder
* Search within zip files optionally
* Search content of zipped XML files (*.*x office files, and iWork(R) files)

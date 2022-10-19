### Things that can be cleaned
 - 1: Random punctuation in places that don't need it.
	 - Specifically regarding square brackets and semicolons. They show up in random locations but are not necessary and do nothing but pollute the dataset.
	 - The semicolons in `physical_description` and `notes` should be kept as is as they denote breaks between list elements, however there are a lot of semicolons in random places in the other columns that should be removed.
	 - These entities should be cleaned as their presence makes it more difficult to read and compare data. An example of this is where the event of one tuple is "DINNER", and the event of another tuple is "[DINNER]", these both have the same events and should be formatted the same way to reflect this. Cleaning these entities will make them more clear without erraneous punctuation marks, make them easier to group together as identical elements will be formatted the same way, and will be easier to analyze and compare different rows that have the same elements.
	 - Clean them in openrefine, simple "find and replace" commands in every column minus the two listed above.
		 - Column Name -> Edit Cells -> Replace, finding `\[|\]|;` as a regex and replacing it with nothing
		 - Make sure to not do this with `physical_description` and `notes`, each other column will have to be handled manually due to this
	 - Actually going to do this in excel because excel is a piece of shit when it comes to dates
			 - Select each column not including `physical_description` and `notes`
		 - Ctrl-H to open the replace menu, find `[` and replace it with nothing
		 - Do the same for `]` and `;`
		 - Profit
 - 2: `name`, `keywords`, `language`, and `location_type` are all empty columns
	 - These empty columns can be removed. Zero entries have any of these fields populated. All that they do is clutter up the data with random empty columns, making it more difficult to read, particularly as there are so many other fields that could be important to a user. Additionally, if you are taking a look at a small amount of rows at once, the existence of these columns implies that other rows will have that data available and that the current rows are not complete.
	 - This should be easy to do in OpenRefine
		 - Column Name -> Edit Column -> Remove This Column
 - 3: Some unknown fields are labelled with `?`, others are left blank
	 - These entities consisting of "?" should simply be deleted as their presence adds nothing to the dataset but inconsistency. There are many blank fields that denote information that is not available, two different formats to explain the same thing is unnecessary and confusing
	 - Removal of these entities should help with analysis by reducing confusion, making the format of unknown fields consistent, and make it obvious when a field is unknown as blank fields jump out more than fields populated by a single character (*elaborate*).
	 - Do this in excel, need to only clear cells that only contain `?`
		 - Ctrl-H -> Find "?" and replace with nothing -> Check off "Match entire cell contents" -> Click "Replace All"
 - 4: Break up `physical_description` and `notes` into multiple columns?
	 - Compare to the first problem, if we do this then we can also remove the semicolons, it should automatically remove them when we split by that character? (specifically "; ")
	 - It should be useful to break these two fields into multiple columns. Both of them consistently have 2+ entries separated by semicolons, which makes it difficult to read and understand the data provided. By separating each entry into its own column, it will become clearer and easier to compare the different tuples. As it is now, users have to read the entire line of notes/physical descriptions in order to find specific ones that they are looking for as it is just a line of capitalized text. By breaking it into their own columns you are able to skim those fields and quickly find what you are looking for.
	 - OpenRefine
		 - Column Name -> Edit Column -> Split into several columns -> Separator should be "; "
 - 5: Differing formats of dates
	 - It looks like there are two different formats of dates used in the dataset. These should be made consistent to use the same date format to prevent confusion. The current date formats are not clear to readers when they look over multiple tuples with different formats, and there are potential issues if the formats are using MM/DD/YYYY or DD/MM/YYYY. Additionally, leading zeros should be added to the month and day sections of the dates, to make each date uniform and easier to compare with one another as the length of the dates will all be consistent (8 numbers)
	 - This is apparently fucking impossible to do in excel
	 - In OpenRefine:
		 - Date -> Edit Cells -> Common Transforms -> To date
		 - Date -> Edit Cells -> Common Transforms -> To text
			 - This step is necessary as it Dates seem to store additional text than what is displayed
		 - Date -> Edit Cells -> Replace, replacing "T00:00:00Z" with nothing
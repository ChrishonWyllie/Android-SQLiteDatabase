# SQLiteDatabase
Quick example showcasing Android's SQLiteDatabase. Used to permanently store data on the device

No pictures are needed for this one. Just some code here...

Of course, you may want to actually do something with the data returned from the SQLiteDatabase and not just simply print it out. For example, perhaps you may want to save some information about the currently signed in user such as their username, email address, etc. or some data about saved games such as high score, last action completed...

I personally use this to save data that I know to be constant such as the aformentioned currently signed in user. Instead of constant requests to your database of backend for information about the signed in user, store a copy on the device and update it whenever the user updates their information. Then you can simply display this data in say, the profile activity or edit-profile activity.

// Wrap in try-catch block

        try {

            // Create a new Database called "Users" or open this database if this already exists.
            // If this is run twice, it will create the database the first time and open it for each subsequent run
            SQLiteDatabase database = this.openOrCreateDatabase("Users", MODE_PRIVATE, null);

            // Create a new table in the "Users" table called "users"
            // This SQL statement guards against multiple runs by only creating the table if it does not already exist.
            database.execSQL("CREATE TABLE IF NOT EXISTS users (userId INTEGER PRIMARY KEY, username VARCHAR, realname VARCHAR, age INTEGER(3))");



            // C.R.U.D. = Create, Read, Update, Delete
            // Commonly referred to when handling data from a database

            // Create some rows for this table
            // WARNING: This will create duplicate values (Although with unique ids)
            // Comment out after first run
            database.execSQL("INSERT INTO users (username, realname, age) VALUES ('BigManJack', 'Jack Smith', 21)");
            database.execSQL("INSERT INTO users (username, realname, age) VALUES ('JasonBourne', 'David Webb', 45)");
            database.execSQL("INSERT INTO users (username, realname, age) VALUES ('Jay-Z', 'Sean Carter', 46)");
            database.execSQL("INSERT INTO users (username, realname, age) VALUES ('The Rock', 'Dwayne Johnson', 39)");


            // Update
            //database.execSQL("UPDATE users SET username = 'JackTheRipper' WHERE realname = 'Jack Smith'");
            // Or..
            //database.execSQL("UPDATE users SET username = 'JackTheRipper' WHERE id = 1");


            // Delete
            database.execSQL("DELETE FROM users WHERE username = 'JackTheRipper'");
            // Or..
            //database.execSQL("DELETE FROM users WHERE id = 1");






            // Read. Now we loop through all the data...

            // Create a Cursor object that will be used to lopp through the table
            // Run a query to SELECT (* = ALL) FROM users table
            Cursor c = database.rawQuery("SELECT * FROM users", null);

            // Get the column number of each column in the table
            int usernameIndex = c.getColumnIndex("username");
            int realnameIndex = c.getColumnIndex("realname");
            int ageIndex = c.getColumnIndex("age");


            // begin at the first object in the query
            c.moveToFirst();

            // While the Cursor loops through the data, print out each object in every row
            while (c != null) {


                // Get each column object
                String username = c.getString(usernameIndex);
                String realname = c.getString(realnameIndex);
                int age = c.getInt(ageIndex);

                System.out.println("username: " + username);
                System.out.println("realname: " + realname);
                System.out.println("age: " + Integer.toString(age));

                // Go to the next row
                c.moveToNext();

            }



        } catch (Exception e) {
            e.printStackTrace();
        }

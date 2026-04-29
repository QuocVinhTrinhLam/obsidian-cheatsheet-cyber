## The following code is vulnerable to SQL injection because the user input is concatenated directly into the query:

`String query = "SELECT * FROM products WHERE category = '"+ input + "'"; Statement statement = connection.createStatement(); ResultSet resultSet = statement.executeQuery(query);`

## You can rewrite this code in a way that prevents the user input from interfering with the query structure:

`PreparedStatement statement = connection.prepareStatement("SELECT * FROM products WHERE category = ?"); statement.setString(1, input); ResultSet resultSet = statement.executeQuery();`




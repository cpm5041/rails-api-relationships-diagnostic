# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
Join tables are useful so you do not have the same data in different places.
This improves code quality so you do not have to update the same thing
in multiple places.  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
  Profiles
    given_name
    surname
    email

  Movies
    title
    release_date
    length

  Favorites
    title
    profile_id
    movie_id

  A profile has a one to many relationship with favorites, favorites would
  join profiles and movies, and there would be a ony to many relationship
  between movies and favorites.
  To draw this, there would be the crows feet on both sides of the Favorite ERD
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
  has_many :favorite
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
  has_many :favorite
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
  belongs_to :movie, inverse_of: :favorite
  belongs_to :profile, inverse_of: :favorite
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
You can customize the JSON that you want returned  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
  attributes :given_name :surname :email
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
bin/rails generate scaffold Favorites movies:references profiles:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
This allows us to remove the association of a relationship when one of
the entities is deleted.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
The doctors to patients relationship comes to mind. You would want an
association between one doctor to many patients (primary care), as well as
many patients to many doctors (list of appointments). In reality, most people
see multiple doctors and doctors see many patients so this would make sense.
  ```

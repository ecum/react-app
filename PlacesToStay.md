# Introduction


The "Places to Stay" project is a web application designed to allow users to search for, book, and manage accommodation. It provides a seamless interface for users to explore various types of accommodations, including hotels, hostels, and campsites, based on their preferences such as location, accommodation type, and availability. The platform also supports a booking management system where users can check the availability of rooms, book accommodation for specific dates, and view their booking history. Additionally, it provides administrative functionalities, enabling administrators to manage accommodation listings, view all bookings, and control the availability of rooms.
In this report the technical aspects of developing the project have been explored using React, Next.js, and Firebase. React provides a robust client-side framework, allowing to create an interactive and dynamic user interface. Next.js, on the other hand, offers server-side rendering and server form actions support, making it easier to manage server-side operations such as handling forms and authentication. Firebase acts as the back-end service, providing data storage, user authentication, and real-time database capabilities, which are essential for managing accommodation listings, bookings, and user information.
The development process of this project includes designing a detailed architecture that integrates client-side and server-side components, implementing the search and booking functionalities, and incorporating user authentication. The goal of this report is to outline the full architecture, explain the implementation of the various components, and discuss the rationale behind the design choices made during the development of the "Places to Stay" project.

## Form handling pattern
Next.js allows the standard html attribute action instead to be an url, which handles the form on submission, to point to a server-side acync function. This function receives as an argument a form data object, containing the user input as entries with name and value of fields. On the other hand, the useFormState React hook is used to provide feedback. On its initialization, it returns a state object and a modified server function to be used in the html action attribute. The original function needs to be modified to take two arguments, the first is the previous state of the form and the form data is the second argument. The return object from the action function is accessible in the UI component. This is used in the project as a feedback in case of validation issues, errors occur in the back-end, messages on successful operations (data mutations), or for data fetching (like in search component).




## Scenario Analysis
The scenario provided for the "Places to Stay" project highlights several core functionalities that need to be implemented for both regular users and administrators. These include searching for accommodations, booking accommodations, managing bookings, and administrative control over listings. Let’s analyze the major functionalities:

### User Functions
Search for Accommodation: Users can search for accommodations based on type (e.g., hotel, hostel, campsite) and location. This functionality is vital for enabling users to filter results and find a place that meets their specific requirements.
Book Accommodation: After searching, users should be able to book accommodation by specifying the date, number of rooms, and number of nights. This feature needs to ensure that rooms are available for the specified date, and once a booking is made, the system should reduce the available rooms for that accommodation.
View Booking History: Users can view a list of their past bookings. Each booking includes details such as the accommodation name, location, date, and number of rooms booked. This functionality helps users keep track of their reservations.
Edit and Cancel Bookings: Users can edit their bookings by changing the date or the number of rooms/nights, as well as cancel bookings. The system should ensure availability before modifying any details, and when a booking is canceled, the availability of rooms should be updated accordingly.

### Admin Functions
Add and Edit Accommodation: Admin users can add new accommodation listings, including the accommodation type, location, and availability information. They should also be able to edit existing listings.
View All Bookings: Administrators can view a list of all bookings made by users, which helps monitor the activity on the platform.
Manage Bookings: Admins should have the ability to edit or cancel any user’s booking. This ensures that they can intervene if there are any issues with bookings or accommodations.

### Architecture Overview
The architecture of the "Places to Stay" project consists of three primary layers: the client-side, server-side, and data storage. The client-side handles the user interface and interactions, the server-side handles business logic and API calls, and the data storage (Firebase) manages persistent data.
Client-Side Components

The client-side of the application is built using React and Next.js. These frameworks allow us to create reusable components, handle form submissions, and manage the user experience efficiently. The primary components include:

Search Form Component: Allows users to search for accommodations based on type and location. This component interacts with Firebase to fetch the list of accommodations that match the user’s query.

Booking Form Component: Once the user selects an accommodation, they are presented with a booking form to specify the date, number of rooms, and nights. This form interacts with the server-side to process the booking and update the availability of rooms.

Edit Booking Component: Allowing them to view booking details and manage their reservations.

## Server-Side Components

Booking History Component: Displays a list of past bookings made by the user.
Admin Dashboard Component: Accessible only to admin users, this component allows administrators to add new accommodations, edit existing ones, and manage user bookings.

### Server-Side Actions
The server-side functionality is handled using Next.js server actions, which provide the business logic for handling form submissions, user authentication, and database interactions. The primary server-side operations include:

Accommodation Search Action (searchAction.js): This function queries Firebase to retrieve accommodations based on user input (type and location). The results are then sent back to the client-side for rendering.

Booking actions (newBookingFormAction.js, editBookingFormAction.js): Handles booking requests from users. This actions checks if the requested accommodation has enough available rooms on the specified date and, if successful, updates the room availability and stores the booking information in Firebase.

Authentication actions: Manages user login and registration using Firebase Authentication.

### Data Storage (Firebase)

Firebase is used as the backend service for the "Places to Stay" project. It manages several aspects of the application:

Firebase Firestore: A NoSQL database used to store accommodation listings and bookings. Each accommodation document includes information such as the type, location, and available rooms for each date. Booking documents store user-specific data such as the accommodation ID, number of rooms, date, and number of nights booked.

Firebase Authentication: Provides secure user authentication for login and registration. Firebase Authentication allows us to create different user roles (e.g., regular users and admins) and manage their access to certain functionalities.

### Detailed Implementation of Components
### Search Functionality
   
The search functionality allows users to filter accommodations by type and location. This is implemented using a form where users select the accommodation type (hotel, hostel, etc.) and input the desired location.
   UI Elements
   Accommodation Type: Allows the user to select the type of accommodation.
   Input Field for Location: Accepts the location as a string.
   
Search Button: Submits the search request.
   
Data Handling
   
User Input: Common form handling pattern is used. “result” key in the formState object  contains the fetched search data.
  
 Firebase Query: When the form is submitted, the search parameters are sent to Firebase Firestore. A query is made to fetch accommodations that match the type and location. The results are then displayed on the UI.

### Booking Functionality
   Users can book accommodations by selecting the date, number of rooms, and nights. This component ensures that rooms are available for the selected date and then updates the availability after booking.
   
UI Elements
   
A form allows users to:
   
- Date Picker:  Selects a booking date.
   
- Input for Number of Rooms: Specifies the number of rooms the user wants to book.
   
- Input for Number of Nights: Specifies the duration of the stay.
   
- Submit Button: Submits the booking request.
   
Data Handling
   
- User Input: Follow the common form pattern.
   
- Firebase Update: When the booking is made, the system checks if there are enough available rooms on the selected date. If available, it updates the availableRooms field for that accommodation in Firebase and creates a new document for the booking in bookings collection.
### Booking History
   The booking history page lists all previous bookings made by the user. Each booking includes the accommodation name, date, number of rooms, and nights. The user can also click on a booking to view full details or cancel/edit it.
   
#### UI Elements

   - List of Bookings: Displays the user’s bookings in a card or list format.
   - Details Button: The name of the accommodation for the booking is made as a link to the full details of the booking.

#### Data Handling

- Data fetching: A server component is used. The bookings are fetched direct in the component.

- Firebase Read: The system queries the bookings collection to retrieve all bookings for the current user.

### Security and Authentication
Security is a crucial part of the "Places to Stay" project, particularly when dealing with user data and booking information. Firebase Authentication is used to handle user registration, login, and authentication.

#### User Authentication

- Registration and Login: Firebase Authentication provides built-in support for user registration and login with email and password. After authentication, a user session is maintained to ensure that only authenticated users can access certain pages (like booking history).
- Admin Role: Admin users are distinguished by adding an additional collection. It holds the admin user ids. This allows the system to grant admin privileges such as viewing all bookings and managing accommodations. Admin dashboard is available on route “/admin”. There is no visual link to it.

#### Access Control
Role-Based Access Control (RBAC): The system implements access control by checking the user’s role before allowing access to specific features. For example, only admin users can add new accommodations or view all bookings.

## Images:
The Image component, provided by Next.js, is used to optimize performance. It makes lazy image loading by default. The images are stored in public/image folder. The image uploading functionality is not implemented in the app at this point. The next.config.mjs is modified in the way that allows app to use images from any sources. The admin users can provide absolute url to the image and do not need to copy it locally.
```images: {
remotePatterns: [
    {
        protocol: 'https',
        hostname: '**', // Allows all hosts
        },
    ],
},
```

## Conclusion

The "Places to Stay" project successfully meets the outlined requirements by providing a user-friendly platform for searching, booking, and managing accommodation. The integration of React for front-end development, Next.js for server-side rendering and API management, and Firebase for real-time data storage and authentication ensures a scalable and efficient application.
Key accomplishments include:

- User Experience: Users can easily search for accommodations and make bookings in a few clicks.
- Real-Time Data Management: With Firebase, the availability of rooms is updated in real-time, preventing overbooking.
- Security: Firebase Authentication provides secure login and registration functionality, ensuring that only authorized users can access sensitive data.
- Admin Control: Admins can manage accommodations and monitor user bookings effectively.

In the future, the platform could be expanded to include additional features such as payment integration, user reviews, and more advanced search filters to enhance the user experience further. Additionally, mobile responsiveness and accessibility can be improved to ensure that the platform caters to a broader audience.

In conclusion, "Places to Stay" provides a solid foundation for a full-fledged accommodation booking system and demonstrates the effective use of modern web technologies to create a dynamic, secure, and scalable web application.

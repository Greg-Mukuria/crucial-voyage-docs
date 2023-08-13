---
title: Current Build (Version 1)
createdAt: '2023-8-5'
---

To be deprecated on 7th Aug 2023 for V2

Our team at Xcelero has been tasked with the creatinon of a fullstack web infrustructure for Crucial Voyages LLC. The task will be divided into two sections, the frontend and the backend which will be connected using N2IR system links, so as to simplify the build.

The designs will be linked below to show what out initial ideas were and what we settled for.

The build components will be listed here 
- Javascript.
- Ruby.
- Node JS.
- PostgreSQL

# Version 1
This is the current build of the Crucial Voyages API routes
## Creating a User Account and authentication by Id

The creation of user accounts is very simple and the body reference is as below

    Type >>>>
    req: Request
    Method: POST

    - {
    "body": {
        "name": "Boris Johnson",
        "email": "borisjohnson@example.com"
        "password": "greatBritainForever"
    }
    }

The end point that will receive the data and give a success response
- The api endpoints will look like this
    - /api/register
    Type <<<<
    res: Response 
    Method: GET

    - {
    "response": {
        "body": "Successfully Created. Status: 200 OK"
    }
    }

Note: Passwords are Hashed for security reasons.

## Creating a listing by Id
## Creating a New Listing (POST Request)

This module handles the creation of a new listing through a POST request. It performs authentication to ensure the user is logged in, processes the incoming data, and saves the listing to the database.

### Dependencies

- `next/server` library
- `@/app/libs/prismadb` module (Assuming this is your Prisma database instance)
- `@/app/actions/getCurrentUser` module

End Point 
 - /api/create-listing
    Type >>>> 
    res: Response
    Method: POST


### Import

```javascript
import { NextResponse } from "next/server";
import prisma from "@/app/libs/prismadb";
import getCurrentUser from "@/app/actions/getCurrentUser";


   

    export async function POST(request: Request) {
  try {
    const currentUser = await getCurrentUser();

    if (!currentUser) {
      return NextResponse.error();
    }

    const body = await request.json();
    const {
      title,
      description,
      // ...
    } = body;

    Object.keys(body).forEach((value: any) => {
      if (!body[value]) {
        return NextResponse.error();
      }
    });

    const listing = await prisma.listing.create({
      data: {
        title,
        description,
        // ...
      },
    });

    return NextResponse.json(listing);
  } catch (error) {
    console.error('Error creating listing:', error);
    return NextResponse.error();
  }
}
```

## Usage
Make sure you have imported the necessary modules.
Use the /api/create-listing endpoint with a POST request to create a new listing.
Include the required data in the request body, such as title, description, etc.
Ensure the user is authenticated before making the request.
Handle any errors that might occur during the creation process.
Notes
This module assumes that you have implemented the authentication logic in the getCurrentUser module and the Prisma database instance in the @/app/libs/prismadb module.
Adjust the property names according to the actual structure of your data.
Error handling in this snippet is basic. Depending on your requirements, you might need more detailed error handling.

Certainly, here's a concise and copy-pasteable version of the documentation snippet for the provided file:

## Creating a Reservation for a Listing (POST Request)

This module handles the creation of a reservation for a listing through a POST request. It performs user authentication, validates incoming data, and updates the corresponding listing with the reservation details.

### Dependencies

- `next/server` library
- `@/app/libs/prismadb` module (Assuming this is your Prisma database instance)
- `@/app/actions/getCurrentUser` module

### Import

```javascript
import { NextResponse } from "next/server";
import prisma from "@/app/libs/prismadb";
import getCurrentUser from "@/app/actions/getCurrentUser";
```

### Endpoint

- **URL**: `/api/create-reservation`
- **Method**: POST

### Request

- **Request Object**: `request` (Express.js Request object)

### Route Implementation

```javascript
export async function POST(request: Request) {
  try {
    const currentUser = await getCurrentUser();

    if (!currentUser) {
      return NextResponse.error();
    }

    const body = await request.json();
    const {
      listingId,
      startDate,
      endDate,
      totalPrice,
    } = body;

    if (!listingId || !startDate || !endDate || !totalPrice) {
      return NextResponse.error();
    }

    const listingAndReservation = await prisma.listing.update({
      where: {
        id: listingId,
      },
      data: {
        reservations: {
          create: {
            userId: currentUser.id,
            startDate,
            endDate,
            totalPrice,
          },
        },
      },
    });

    return NextResponse.json(listingAndReservation);
  } catch (error) {
    console.error('Error creating reservation:', error);
    return NextResponse.error();
  }
}
```

### Usage

1. Make sure you have imported the necessary modules.
2. Use the `/api/create-reservation` endpoint with a POST request to create a reservation.
3. Include the required data in the request body, such as `listingId`, `startDate`, `endDate`, and `totalPrice`.
4. Ensure the user is authenticated before making the request.
5. Handle any errors that might occur during the reservation creation process.

### Notes

- This module assumes that you have implemented the authentication logic in the `getCurrentUser` module and the Prisma database instance in the `@/app/libs/prismadb` module.
- Adjust the property names according to the actual structure of your data.
- Error handling in this snippet is basic. Depending on your requirements, you might need more detailed error handling.
```

Please note that deletion is upto a DELETE method as delete function, PATCH method as a update function but the bodies of both requests and responses will be exactly the same.




# Version 2

Note: This is version 2 of the api endpoints ( Not Applicable: Development Stalled )
The build idea will have the following things in place by second draft. 
- Crud functions for creating trips.
    ## Create a trip
        - Creation of a trip means that the user can create a trip to a destionation and choose (via a graphics user interface with cards or map system). This will allow the user to choose between solo travel, Family Pack, For Organisations. 

        - The next aspects are forms that can collect data from the user and submit it to an api end point. Prefereably 
            - /api/v2/solo-travel/new-trip/trip-iD
            - /api/v2/family-pack/new-trip/trip-iD
            - /api/v2/for-organizations/new-trip/trip-iD

        We can create a timer element that will count down to the date of expiry.
        The nuber of participants travelling will be noted and data collected will be stored for further transmissions

    ## Delete a trip
        - Simply the ability to delete the trip from the users account in all modes of booking logic. 

        - The api endpoints will look like this
            - /api/v2/solo-travel/discard-trip/trip-iD
            - /api/v2/family-pack/discard-trip/trip-iD
            - /api/v2/for-organizations/discard-trip/trip-iD
        
    ## Read a trip
        - Likewise for the ability to read any data pertaining to the trip or trips created. Here is where the ability to delete and update will be used.
        - The api endpoints will look like this
            - /api/v2/solo-travel/review-trip/trip-iD
            - /api/v2/family-pack/review-trip/trip-iD
            - /api/v2/for-organizations/review-trip/trip-iD

    ## Update a trip
        For updating the trips a user has created, the enpoints will be
        - The api endpoints will look like this
            - /api/v2/solo-travel/change-trip/trip-iD
            - /api/v2/family-pack/change-trip/trip-iD
            - /api/v2/for-organizations/change-trip/trip-iD

    ## Payments and Billing 
        For updating the trips payments a user has created, the enpoints will be
        - The api endpoints will look like this
            - /api/v2/solo-travel/LandedPayments/paymentId
            - /api/v2/family-pack/LandedPayments/paymentId
            - /api/v2/for-organizations/LandedPayments/paymentId

    ## Maps, Logistics and Services
        Maps used in the project are dynamic in version 1 but implementation of a GoogleMaps or AppleMaps api 
        For simulation purposes this elements will not be publicly available
        - The api endpoints will look like this

            - /api/v2/solo-travel/mapId/experienceID
            - /api/v2/family-pack/mapId/experienceID
            - /api/v2/for-organizations/mapId/experienceID

    - Trips include in-bound, outbound and domestic trips. 

    ## Outbound Trips
    Outbound trips will be categorized on its own for simplicity sake. These will include world-wide places of travel. 

    ## Domestic Trips
    Domestic trips will be categorized on its own for simplicity sake. These will include domestic places of travel.

    Protected Routes will be present. These routes will be poised for super administrative access. 
    Of course master passwords will conveyed. 
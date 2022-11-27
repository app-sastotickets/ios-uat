
## Sasto Tickets SDK Integration summary ( IOS )

<br>
<br>

Minimum SDK Requirement: IOS 14


#### Steps to add SDK to your project

1. Open your project in Xcode

2. Go to File > Add Packages...

3. In the field Enter package repository URL, enter "https://github.com/app-sastotickets/ios-uat.git"

4. Pick the latest version and Add Package.

#### Initializing SDK

1. Import SwiftUi & Sastotickets in the controller file from where we are going to initilize sastotickets sdk :

```
import SwiftUI
import SastoTickets
        
```

2. Run the following code preferably on button click which will display sastotickets user interface

```
let vc = UIHostingController(rootView:SastoticketsView(clientId: "", clientSecret: "", walletBalance: 0, phone: ""){response, error in
    if error != nil {
        //Handle error
    }
    if response != nil {
        //Do something with response
    }
    })
vc.modalPresentationStyle = .fullScreen
present(vc, animated: true)

```


#### Usage Example
ViewController.swift

```
import UIKit
import SwiftUI
import SastoTickets



class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        let button = UIButton(frame: CGRect(x: 0, y: 0, width: 220, height: 50))
        view.addSubview(button)
        button.center = view.center
        button.setTitle("Show sdk", for: .normal)
        button.backgroundColor = .systemRed
        button.addTarget(self, action: #selector(didTapButton), for: .touchUpInside)
        
        
    }
    
    @objc func didTapButton(){
        let vc = UIHostingController(rootView:SastoticketsView(clientId: "", clientSecret: "", walletBalance: 0, phone: ""){response, error in
            if error != nil {
                //Handle error
            }
            if response != nil {
                //Do something
            }
        })
        vc.modalPresentationStyle = .fullScreen
        present(vc, animated: true)
        
    }

}
        
```

#### Flight Booking & Ticketing Workflow
Generally flight ticketing will consist following steps


```
Step 1. Flight Search
Step 2. Availability
Step 3. Reservation
Step 4. Payment
Step 5. Issue Ticket
```
For more details regarding the API, refer the following documentation
https://sastotickets-integration-b 2 b.web.app

SDK will handle the 1. Flight Search, 2. Availability & 3. Reservation part and the 4.

Payment and 5. Issue Ticket should be implemented at your end.

After the completion of 3. Reservation SDK will provide a callback with necessary payload required to
call 5. Issue Ticket API

```
Payload example from the SDK
{
"bookingSummary": {
      "bookingReferenceID": "10f47dd7-2a6f-41c8-a3c0-9b4668249c88",
      "PNR": "6W6P48",
      "tripType": "oneway",
      "currency": "NPR",
      "passengers": [
        {
          "passenger_id": "ST308",
          "passenger_type": "ADT",
          "passenger_name": "Maharjan Rabi Kumar"
        }
      ]
    }
"bearerToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczpcL1wvMjAxOWRldi1h
cGkuc2FzdG90aWNrZXRzLmNvbVwvYXBpXC9iMmJcL3YxXC9nZXQtdG9rZW4iLCJpYXQiOjE2NT
U4MTc0NTksIm5iZiI6MTY1NTgxNzQ1OSwianRpIjoicHVOczhPRkpRMnZMS0xhMyIsInN1YiI
MTQsInBydiI6ImYyN2RiZjM3OTBlN2IxZmNhOGRmYjZjODhlZjk1YWY5MDQ2YTg4ZmMifQ.kXY
TKzP__Ya-CpP5vYRuzVB1JVbTUaQrPOi0NEBVRQY"
}
```
#### Your App

You should now implement the payment part and debit the customer's wallet/account and after the completion of payment should call the Sasto Tickets Ticket Issue API for issuing ticket and getting
ticket details in the response.

```
Payload example for calling the Sasto Tickets ticket issue API

Endpoint: https://2019dev-api.sastotickets.com/api/b2b/v1/issue-ticket
Method: Post
Header: Authorization: Bearer
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczpcL1wvMjAxOWRldi1hc
Gkuc2FzdG90aWNrZXRzLmNvbVwvYXBpXC9iMmJcL3YxXC9nZXQtdG9rZW4iLCJpYXQiOjE2NTU
4MTc0NTksIm5iZiI6MTY1NTgxNzQ1OSwianRpIjoicHVOczhPRkpRMnZMS0xhMyIsInN1YiI6M
TQsInBydiI6ImYyN2RiZjM3OTBlN2IxZmNhOGRmYjZjODhlZjk1YWY5MDQ2YTg4ZmMifQ.kXYT
KzP__Ya-CpP5vYRuzVB1JVbTUaQrPOi0NEBVRQY
Content-Type: application/json
Body
{
"bookingReferenceID": "9a237fa9-d970-4457-8535-7440eeac06ac",
"PNR": "6W6P48",
"tripType": "oneway",
"currency": "NPR",
}

Response Sample from Sasto Tickets
{
"success": true,
"data": {
"ticketSummary": {
"ticketReferenceID": "fc2e2b9f-d5a8-4c28-a9c4-387354a88d6a",
"PNR": "65NOLL",
"tripType": "oneway",
"currency": "NPR",
"issued_date": "Wed, Jun 22, 2022"
},
"flightSummary": {
"airline_confirmation_code": "65NOLL",
"departure_date": "2022-07-15",
"departure_time": "10:15 AM",
"arrival_date": "2022-07-15",
"arrival_time": "11:50 AM",
"total_flying_hours": "04 Hr 20 Min",
"depart_city_airport_code": "KTM",
"depart_city_airport_name": "Tribhuvan Intl Airport",
"depart_city_name": "Kathmandu City",
"depart_city_country": "Nepal",
"carrierCode": "QR",
"carrierLogo": "https://sastotickets-flights.s3.ap-southeast-
1.amazonaws.com/airlines/1633428326-1898803185.png",
"carrierName": "Qatar Airways",
"dest_city_airport_code": "DOH",
"dest_city_airport_name": "Hamad Int’l Airport",
"dest_city_name": "Doha",
"dest_city_country": "Qatar",
"flight_segments": [
{
"carrierCode": "QR",
"carrierName": "Qatar Airways",
"carrierLogo": "https://sastotickets-flights.s3.ap-
southeast-1.amazonaws.com/airlines/1633428326-1898803185.png",
"flight_number": "653",
"flight_equipment": "Airbus A330-300",
"cabin_class": "M",
"seg_departure_date": "2022-07-15",
"seg_departure_time": "10:15 AM",
"seg_arrival_date": "2022-07-15",
"seg_arrival_time": "11:50 AM",
"seg_total_flying_hours": "04:20:00",


"seg_departure_airport_code": "KTM",
"seg_departure_airport": "Tribhuvan Intl Airport",
"seg_departure_city": "Kathmandu City",
"seg_departure_country": "Nepal",
"seg_departure_terminal": "I",
"seg_arrival_airport_code": "DOH",
"seg_arrival_airport": "Hamad Int’l Airport",
"seg_arrival_city": "Doha",
"seg_arrival_country": "Qatar",
"seg_arrival_terminal": "",
"seg_connection_time": false
}
]
},
"baggageDetails": {
"cabin": "7 Kg",
"baggage": "30 Kg"
},
"passengers": [
{
"passenger_type": "ADT",
"passenger_name": "Maharjan Rabi Kumar",
"eticket_number": "157-6930434619",
"PNR": "65NOLL",
"airlines_confirmation": "65NOLL"
}
]
}
}

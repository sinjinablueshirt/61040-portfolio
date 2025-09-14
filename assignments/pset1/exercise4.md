# Exercise 4
[back to table of contents](/assignments/pset1/contents.md)

## Concept 1: Billable Hours Tracking

**concept** billableHoursTracking[Time]

**purpose** record working hours of employees

**principle** once registered, the employee clocks in at the beginning of every shift, selecting a project and describing work to be done, then they clock out when they stop working.

**state** <br>
&nbsp;&nbsp;a set of Employee with<br>
&nbsp;&nbsp;&nbsp;&nbsp;a id String <br>
&nbsp;&nbsp;&nbsp;&nbsp;a shifts set of Shift <br>
&nbsp;&nbsp;&nbsp;&nbsp;a startTime Time <br>
&nbsp;&nbsp;&nbsp;&nbsp;a currentProject String <br>
&nbsp;&nbsp;&nbsp;&nbsp;a currentDescription String <br>
&nbsp;&nbsp;&nbsp;&nbsp;a working Flag <br>

&nbsp;&nbsp;a set of Shift with<br>
&nbsp;&nbsp;&nbsp;&nbsp;a duration Float <br>
&nbsp;&nbsp;&nbsp;&nbsp;a project String <br>
&nbsp;&nbsp;&nbsp;&nbsp;a description String <br>

**actions** <br>
&nbsp;&nbsp;register(id: String): (employee: Employee) <br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** no employee with the same id exists <br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** creates a new non-working employee with id with no shifts and returns it.

&nbsp;&nbsp;clockIn(employee: Employee, time: Time, project: String, description: String) <br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** employee exists and is currently not working. <br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** saves time as the startTime. saves currentProject as project and currentDescription as description. designates employee as working

&nbsp;&nbsp;clockOut(employee: Employee, endTime: Time) <br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** employee exists and is currently working <br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** create and save a new shift containing the currentProject and currentDescription to the employee's shifts. The duration of this shift should be min(endTime-startTime, 8). designates employee as not working



## Concept 2: Electronic Boarding Pass

**concept** electronicBoardingPass[BoardingPass, Flight]

**purpose** electronically display and update information on a user's boarding pass

**principle** once a user has registered and booked a flight, they may add a boarding pass to their wallet that gets updated as updated flight information comes in.

**state** <br>
&nbsp;&nbsp;a set of Passenger with <br>
&nbsp;&nbsp;&nbsp;&nbsp;an id String <br>
&nbsp;&nbsp;&nbsp;&nbsp;a flights set of Flight <br>
&nbsp;&nbsp;&nbsp;&nbsp;a wallet set of BoardingPass <br>

**actions** <br>
&nbsp;&nbsp;register(id: String): (passenger: Passenger)<br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** a passenger with id doesn't already exist<br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** creates and saves a new passenger with id and no flights or boarding passes

&nbsp;&nbsp;bookPassenger(id: String, flight: Flight)<br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** a passenger with id exists and is not already booked for flight<br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** books the passenger with id for the flight. Creates and saves an electronic boarding pass to be added to the passenger's digital wallet.

&nbsp;&nbsp;updateFlight(flight: Flight)<br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** true<br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** updates the flight information for all passengers booked for flight. for each of those passengers, creates and saves a new boarding pass to go in the passengers' digital wallets.

&nbsp;&nbsp;cancelBooking(id: String, flight: Flight)<br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** a passenger with id is booked for flight <br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** removes passenger from flight. Remove the corresponding boarding pass from the passenger's wallet




## Concept 3: Address Verification

**concept** addressVerification[Address]

**purpose** verify clients against their address information

**principle** once a client registers using their name and address, different companies/businesses can use the information to verify future transactions with the client.

**state** <br>
&nbsp;&nbsp;a set of Client with <br>
&nbsp;&nbsp;&nbsp;&nbsp;a username String <br>
&nbsp;&nbsp;&nbsp;&nbsp;an address Address

**actions** <br>
&nbsp;&nbsp;register(username: String, address: Address) <br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** a client with the same name and address doesn't already exist <br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** creates and saves a new client with username and address

&nbsp;&nbsp;changeAddress(username: String, newAddress: Address) <br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** A client already exists with the same username and a different address <br>
&nbsp;&nbsp;&nbsp;&nbsp;**effects** changes the client's address to newAddress

&nbsp;&nbsp;verify(username: String, address: Address) <br>
&nbsp;&nbsp;&nbsp;&nbsp;**requires** A client named name exists and that same client's address matches address


## Notes

1. for billableHoursTracking, I made it so that a shift can't be longer than 8 hours (arbitrarily) so that if a worker forgot to clock out it would just not track the extra hours.

2. for electronicBoardingPass, I made Flight and BoardingPass generic types that would store whatever information about the flight/boarding pass. I figured this would be the best way because (looking at the article) there's A LOT of information on boarding passes and flights and it would be better to abstract it away for the purpose of this concept.

3. For addressVerification, I structured it in nearly the same way as password authentication but with a generic type for the address. I thought this would be fitting given that different applications may want different types/parts of addresses to authenticate. This lets the concept be useful in many contexts. I framed it such that if a service does the verify action and succeeds then the address matches.

4. For addressVerification, it is important to note that though it is similar strucurally to passwordAuthentication, its purpose is for companies/businesses to verify clients rather than clients verify themselves.

[back to table of contents](/assignments/pset1/contents.md)

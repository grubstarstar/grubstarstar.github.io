messi = new Footballer({dribbling:10})

POST /footballer
{
	dribbling: 10
}

{
	id: 1,
	dribbling: 10,
	shooting: 10,
	tackling: 5
}


dribbling =  messi.dribbling

GET /footballer/1
{
	id: 1,
	dribbling: 10,
	shooting: 10,
	tackling: 5
}

response.dribbling


isGoal = messi.shoot("top right")

GET /footballer/1?methodToCall=shoot&args[aim]="top right"

You wouldn't set a property on an object in OOP to perform an operation, so why would you over an API?
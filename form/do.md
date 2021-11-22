
# Suggestions

This section will contains the suggestion on how to do form in a way that it's easier to maintain and read.


## 1. Map api response to view model before processing

When we load data from database, it's better to preprocess the data first before we represent it to UI. For example, if we have the api request as follow:
``` js
formRecordApi.getAll(koFormId(), query,{
    success: function(response){
        koListOfItems(response); //this is using the response right away
    }
})
```
it will be better to extend each of the item first beforehand:
```js
formRecordApi.getAll(koFormId(), query,{
    success: function(response){
        for (var i in response) //we extend first
            extendEntity(response[i])
        koListOfItems(response); 
    }
})

function extendEntity(entity){
    entity.formattedPrice = observable(entity.price.toLocaleString());
    //...other entity properties that required to be exposed.
}
```

## 2: Make use of pureComputed for data formatting

Move as much data massaging to pure computed. Instead of handling in html.

for example, as this code:
``` html
<td>
    <span data-bind="text: moment(sta).format('DD-MM-YYYY')"></span>
</td>
```
we make it into:
``` html
<td>
    <span data-bind="text: formattedSTA"></span>
</td>
```

``` javascript
var formattedSTA = ko.pureComputed(()=> { return moment(sta).format('DD-MM-YYYY'); })
```

## 3. Seperate listing and detail api

It is consider better to have two tier api, of having **listing api** that only return a sub set of the properties and pagination), 
and **detail api** that accepts a given entity id and return the complete entity object.


> Example:
> - **list page**: sending api request to ```/gaterelease```, that will provide the api for ```/gaterelease/{gateReleaseId}``` so we can get the detail of a single gate release
> - **detail**: sending api request to ```/pams``` and we can use ```/pam?gateReleaseId={gateReleaseId}``` to filter the pam by gate release id.

## 4  Refering Parent Scope In Data

Refer to [Overall Suggestion #1](#2-unsafe-and-bad-data-attribute-binding)


## 5. Better client side filtering

Better create a dictionary based on user id first instead of looping one by one each time trying to retrieve user id.

Also the naming is wrong, seems to be ```getUserName``` instead of ```getUser```

``` js

function reindexUser(users){
    var result = {

    };
    ko.utils.arrayForEach(users, function (user) {
        userId = user.userId;
        result[userId] = user;
    });
    koUserList(result);
}

function getUser(userId){
    var userList = koUserList()
    if (userId && userId in userList)
        return userList[userId];
    return null;
}

function getUserName(userId){
    user = getUser(userId);
    if (user)
        return user.displayName;
    return "";
}
```

## 6. Mapping Viewmodel back to service model before posting to service

Its always consider better to remapped view model back to service model before posting back to services
.

During the life time of an object, a given object have have additional attached properties that is not required.
It is consider to be better to only send what is required to prevent side effects and better readability.
1. easier to find what are the payload to be sent in the request
2. the probability of the UI crash due to missing attribute is lower

``` js
list = $.map(list, function (item) {   

    item.packages = $.map(item.packages, function (package) {
        if (package.isSelected) {
            packageCount++;
            
            return {
                hawbNo: package.hawbNo
            };
        }
    })

    if (item.packages.length > 0)
        return item;
});

```
change to:
``` js


list = $.map(list, function (item) {
    //delete item.isExpand; //this one, just remove this line

    item.packages = $.map(item.packages, function (package) {
        if (package.isSelected) {
            packageCount++;
            return {
                hawbNo: package.hawbNo
            };
        }
    })

    if (item.packages.length > 0)
        return item;
});


var data = {
    packages: item.packages,
    driverName: koDriverName(),
    /// and so on
}
formRecord.post(koFormId(), data, {  //change the query to data
    success: function (result) {
        
    },
    fail: function(result){

    }
});

```

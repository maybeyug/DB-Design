# Instagram Thrift Creator Store

Hi!<br>
This is my approach to designing the DB for given task. <br>

[Instagram_thrift_design](./insta_thrift_dbDesign.png)

<br>

## Entity_Relation Diagram 

- products
- thrifted_items
- handmade_item
- customers
- orders
- order_items
- payment
- shipping


### DBML CODE

```title Task01_instagram_thrift

//products
products [icon: product, color:yellow]{
  id serial pk
  name varchar(50) unique not null
  description text not null
  category enum["thrifted","handmade"]
  color varchar(10) 
  price int not null
  createdAt timestamp
  updatedAt timestamp
}

thrifted_items [icon: product, color: yellow]{
  id serial pk
  product_id int fk
  category ['thrifted']
  size char(5) not null //one-size as it's thrifted
  condition text
  quantity int (default 1)

  createdAt timestamp
  updatedAt timestamp
}

handmade_item [icon:product, color: yellow]{
  id serial packing
  product_id int fk
  category ['handmade']
  size enum['s','m','l']
  quantity int 

  createdAt timestamp
  updatedAt timestamp
}

//customers
customers [icon: user, color:blue]{
  id serial pk
  name varchar(50) not null
  email varchar(322) unique not null
  phone char(12) not null
  delviery_address text not null
  city varchar(30) not null
  state varchar(20) not null
  pincode char(8) not null

  createdAt timestamp
  updatedAt timestamp
}

//orders
orders [icon: shopping-bag, color: green]{
  id serial pk
  customer_id int fk

  status enum["packing","shipped","delivered"]
  amount int
  type enum['prepaid','cod'] 
  payment_id int fk 
  
  shipping_id int fk
  
  createdAt timestamp
}

//order_items
order_items[icon: shopping-cart, color:green]{
  id serial pk
  order_id int fk
  product_id int fk
  quantity int
  price int 

}

//payment
payment[icon: money, color:red]{
  id serial pk
  order_id int fk
  customer_id int fk
  txn_ref text not null
  mode enum["upi","card"] 
  date timestamp
}

//shipping
shipping [icon: truck, color: purple]{
  id serial pk
  order_id int fk
  customer_id int fk
  payment_status enum['paid','cod']
  status enum['pending','transit','out_for_delivery','delivered','not_delivered']
  address text not null
  createdAt timestamp
  updatedAt timestamp
}

products.id - thrifted_items.product_id 
// one product id for one thrift item

products.id < handmade_item.product_id
// Reason: one product can have different sizes/colors but they all are same product

orders.id < order_items.order_id
// Reason: one order can have multiple items

customers.id < orders.customer_id
// Reason: one customer can place multiple orders

products.id < order_items.product_id
// Reason: one product_id with multiple items such as same shirt but different colors

payment.order_id - orders.payment_id
// Reason: One payment id for one order

customers.id < payment.id
//Reason: One customer can have multiple payment_id means multiple transaction for multiple orders

customers.id < shipping.id
//Reason: one customer and many placed orders

orders.id - shipping.order_id
//Reason: one order will have one shipping id
```
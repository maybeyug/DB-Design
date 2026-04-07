# Fitness Influencer Coaching Platform

Web Dev Cohort 2026

Hi!<br>
This is my approach to designing the DB for given task. <br>

[Fitness_influencer_coaching_platform](./coaching_platform.png)

<br>

## Entity_Relation Diagram 

- trainer
- customer_account
- customer_details
- check_in_reports
- progress
- plans
- subscription
- payment
- trainer_notes
- session
- consultation

### DBML Code
``` 
colorMode pastel
styleMode shadow
title 02_Fitness_influencer_coaching_platform
 
 // Trainer
 trainer [icon: dumbbell, color: blue]{
  id serial pk
  name varchar(30) not null
  age int not null
  gender varchar(10)
  experience int not null
  instagram_handle string
  youtube_link string
  
  createdAt timestamp
  updatedAt timestamp

 }

 //customer
customer_account [icon: user, color: green]{
  id serial pk
  name varchar(20) not null
  email varchar(322) unique not null
  phone char(12) not null

  active_plan_id int fk
  isSubscribe boolean default false
  subscribe_id int fk

  createdAt timestamp
  updatedAt timestamp
}


customer_details [icon: settings, color: green]{
  id serial pk
  customer_id int fk
  age char(3) not null
  gender varchar(10)
  height int not null
  weight int not null
  diet_prefrence varchar(10) // [veg,veg+egg,non-veg]
  fitness_goal text

  check_in_report int fk

  createdAt timestamp
  updatedAt timestamp
}

check_in_reports [icon: report, color:green]{
  id serial pk
  customer_id int fk
  customer_detail_id int fk

  weight varchar(20) not null
  height varchar(20) not null
  waist varchar(20)
  checked_by_trainer boolean default false
  trainer_feedback text 
  trainer_notes text 
  trainer_id int fk
  weekly_report_attach link

}

 //plans
 plans [icon: document, color: blue]{
  id serial pk
  trainer_id int fk
  name  text not null
  description text not null
  price int
  duration varchar(10)
  customer_id int fk
  has_fiteness boolean default false
  has_diet boolean
  has_session boolean
  has_consultation boolean

 }

customer_account.id < check_in_reports.customer_id: [color: green]
customer_details.check_in_report < check_in_reports.customer_detail_id: [color: green]

trainer.id - check_in_reports.trainer_id: [color: blue]

customer_account.id - customer_details.customer_id: [color: green]

customer_account.active_plan_id <> plans.customer_id: [color: blue]

trainer.id < plans.trainer_id: [color: blue]

 //subscription
 subscription [icon: azure-subscriptions, color:yellow]{
  id serial pk
  customer_id int fk
  amount int 
  duration varchar(20)
  payment_id int fk
  plan_id int fk
  createdAt timestamp
 }

plans.id < subscription.plan_id: [color: yellow]
customer_account.subscribe_id < subscription.customer_id: [color: yellow]

 // payment
payment [icon:money,color: red]{
  id serial pk
  txn_id int fk
  customer_id int fk
  subscription_id int fk
  payment_status varchar(20)
  bank_details vcarchar(20)
  mode_of_payment varchar(20)
  is_auto_pay boolean default false

  createdAt timestamp
}

subscription.payment_id - payment.txn_id
payment.subscription_id - subscription.id: [color: red]
payment.customer_id - customer_account.id: [color: red]

 //trainer_notes
 trainer_notes[icon: notebook,color: Black]{
  id serial pk
  trainer_id int fk
  customer_id int fk
  title text not null
  description text
  is_report_check boolean default false
  createdAt timestamp
  updatedAt timestamp
  
 }

trainer.id < trainer_notes.trainer_id 
check_in_reports.customer_id < trainer_notes.customer_id 

 //session
 session[icon : zoom, color:purple]{
  id serail pk
  trainer_id int fk
  topic text not null
  duration int
  user_limit int
  customer_id int fk
  is_subscribe boolean
  plan_id int fk

  createdAt timestamp
 }

 trainer.id < session.trainer_id : [color: purple]
 session.customer_id <> customer_account.id: [color: purple]
 session.plan_id > plans.id: [color: purple]

 //consultation
 consultation[icon: phone-call, color: Orange]{
  id serial pk
  customer_id int fk
  is_subscribe boolean
  trainer_id int fk
  topic text
  mode varchar(20)
  createdAt timestamp
 }

consultation.trainer_id > trainer.id: [color: orange]
customer_account.id - consultation.customer_id: [color: orange]

//Progress
progress[icon: graph]{
  id serial pk
  customer_id int fk
  trainer_id int fk
  goal_achieved varchar(10)
  duration varchar(20)
  report_id
}
 check_in_reports.id - progress.report_id


```

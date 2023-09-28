const express = require('express');
const bp = require('body-parser');
const ejs = require('ejs');
const axios = require('axios');
const app = express();

app.set('viewengine','ejs');
app.use(bp.json());
app.use(bp.urlencoded({ extended: true }));
 
app.get('/', (req, res) => {
  res.render('index.ejs',{place:'',text:''});
});

app.post('/',(req,res) => {
  const loc = req.body.location;
  axios.get("http://api.weatherapi.com/v1/current.json?key=ff4cc91113fa474cb9c83331232308&q="+loc+"&aqi=no").then((response)=>{
    res.render('index.ejs',{place:response.data.current['temp_c']+" \u00B0"+"C",text:response.data.current.condition['text']});
    console.log(response.data);
  }).catch(err=>console.log(err));
});

app.listen(3000, () => {
  console.log('Server Started');
});

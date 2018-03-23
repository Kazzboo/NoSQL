# NoSQL

LISTE DES REQUÊTES

REQUÊTE “FACILE” :

VOIR LA BD JOLIMENT:

 db.tournage.find().pretty();
 
METTRE EN ÉVIDENCE LES DIFFÉRENTS TITRES DE SHOW FAIT PAR UN RÉALISATEUR DONNÉ

db.tournage.find({'fields.realisateur': /SIMON ASTIER/},{'fields.titre':1, 'fields.realisateur':1});

METTRE EN ÉVIDENCE LES TYPES DE TOURNAGE RÉALISÉS PAR UN ORGANISME DONNÉ

db.tournage.find({'fields.organisme_demandeur':/BIG BAND STORY/},{'fields.type_de_tournage':1, 'fields.organisme_demandeur':1,'_id':0});

REMONTER LES ADRESSES DE TOURNAGES QUI ONT VU RÉALISÉ PLUS D’UN TOURNAGE

$ db.zoneDeTournage.aggregate([{$group : { _id : "$fields.adresse", total : { $sum : 1 } }} ,{ $sort : { total : -1 } }, { $out : "new"}]);
db.new.find({'total': { $gt : 1}})

LISTE DE TOUS LES TYPES DE TOURNAGE:

db.tournage.distinct('fields.type_de_tournage');

LISTE DE TOUS LES RÉALISATEURS :

db.tournage.distinct('fields.realisateur');


REQUÊTE “MOYENNE” :

FAIRE UN DELETE

db.tournage.remove({ 'fields.realisateur': "ARNAULD MERCADIER" },{'fields.realisateur': 1} );

FAIRE UN UPDATE

db.tournage.update( {_id: ObjectId("5ab26fd3c0e9bfe6694e0ca4"), "fields.type_de_tournage": "TELEFILM"}, { $set: { "fields.type_de_tournage": "telefilm"} } );

FAIRE UN INSERT

db.tournage.insert({dataseid :  ‘tournagesdefilmsparis2011’, "recordid" : ‘d77fabba3250022889dacba7429ec107368f0ccf’, fields.tipe_tournage : ‘SERIE TELEVISEE’, fields.organisme_demandeur : ‘CALT PRODUCTION’, fields.adresse : ‘97  BOULEVARD  HAUSSMANN’, fields.date_fin : “2016-03-22”, fields.realisateur : ‘MARINE HUERTAS’, fields.xy : [48.874813,2.31818], fields.ardt : 75008 , fields.titre : ‘CODING LIFE’, fields.date_debut : “2016-03-22”, geometry.type: ‘Point’, geometry.coordinate: [2.31818,48.874813]};


NOMBRE DE TOURNAGES RÉALISÉS PENDANT UN TRIMESTRE

db.zoneDeTournage.find({'fields.date_debut': { $gt : "2016-01-01"},'fields.date_debut': { $lt : "2016-03-01"}  }, {'fields.date_debut':1}).count()

REQUÊTE “COMPLEXE” :

METTRE EN ÉVIDENCE L’ORGANISME AYANT RÉALISÉ LE PLUS TOURNAGES

$ db.zoneDeTournage.aggregate([{$group : { _id : "$fields.organisme_demandeur", total : { $sum : 1 } }} ,{ $sort : { total : -1 } }, { $out : "new"}]);
$ db.new.find({},{ total : 1}).limit(1)

FAIRE UN ENCADREMENT GÉOGRAPHIQUE DES TOURNAGES COMPRIS ENTRE LES COORDONNÉES 

db.tournage.find({'geometry.coordinates' : {$near : [2.320483, 48.876236], $maxDistance : 1/111.12}}).count()

FAIRE UNE LISTE PAR ORDRE DÉCROISSANT DE TYPE DE SHOW DIFFUSÉ EN FONCTION DU NOMBRE 

$ db.zoneDeTournage.aggregate([{$group : { _id : "$fields.type_de_tournage", total : { $sum : 1 } }} ,{ $sort : { total : 1 } }, { $out : "new"}]);
$ db.new.find({},{ total : 1});

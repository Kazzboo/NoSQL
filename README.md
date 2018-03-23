# NoSQL

Liste des requêtes

Requête “facile” :
Voir la BD joliment:
 db.tournage.find().pretty();
Mettre en évidence les différents titres de show fait par un réalisateur donné
db.tournage.find({'fields.realisateur': /SIMON ASTIER/},{'fields.titre':1, 'fields.realisateur':1});
Mettre en évidence les types de tournage réalisés par un organisme donné
db.tournage.find({'fields.organisme_demandeur':/BIG BAND STORY/},{'fields.type_de_tournage':1, 'fields.organisme_demandeur':1,'_id':0});
Remonter les adresses de tournages qui ont vu réalisé plus d’un tournage 
$ db.zoneDeTournage.aggregate([{$group : { _id : "$fields.adresse", total : { $sum : 1 } }} ,{ $sort : { total : -1 } }, { $out : "new"}]);
db.new.find({'total': { $gt : 1}})
Liste de tous les types de tournage:
db.tournage.distinct('fields.type_de_tournage');
Liste de tous les réalisateurs :
db.tournage.distinct('fields.realisateur');


Requête “moyenne” :
Faire un delete
db.tournage.remove({ 'fields.realisateur': "ARNAULD MERCADIER" },{'fields.realisateur': 1} );

Faire un update
db.tournage.update( {_id: ObjectId("5ab26fd3c0e9bfe6694e0ca4"), "fields.type_de_tournage": "TELEFILM"}, { $set: { "fields.type_de_tournage": "telefilm"} } );
Faire un insert
db.tournage.insert({dataseid :  ‘tournagesdefilmsparis2011’, "recordid" : ‘d77fabba3250022889dacba7429ec107368f0ccf’, fields.tipe_tournage : ‘SERIE TELEVISEE’, fields.organisme_demandeur : ‘CALT PRODUCTION’, fields.adresse : ‘97  BOULEVARD  HAUSSMANN’, fields.date_fin : “2016-03-22”, fields.realisateur : ‘MARINE HUERTAS’, fields.xy : [48.874813,2.31818], fields.ardt : 75008 , fields.titre : ‘CODING LIFE’, fields.date_debut : “2016-03-22”, geometry.type: ‘Point’, geometry.coordinate: [2.31818,48.874813]};

		"xy" : [
			48.874813,
			2.31818
		],
		"ardt" : 75008,
		"titre" : "CHEFS - SAISON 2",
		"date_debut" : "2016-03-22"
	},
	"geometry" : {
		"type" : "Point",
		"coordinates" : [
			2.31818,
			48.874813
		]
	},


Nombre de tournages réalisés pendant un trimestre
db.zoneDeTournage.find({'fields.date_debut': { $gt : "2016-01-01"},'fields.date_debut': { $lt : "2016-03-01"}  }, {'fields.date_debut':1}).count()

Requête “complexe” :
Mettre en évidence l’organisme ayant réalisé le plus tournages (AdrienG)
$ db.zoneDeTournage.aggregate([{$group : { _id : "$fields.organisme_demandeur", total : { $sum : 1 } }} ,{ $sort : { total : -1 } }, { $out : "new"}]);
$ db.new.find({},{ total : 1}).limit(1)
Faire un encadrement géographique des tournages compris entre les coordonnées (Cédric)
db.tournage.find({'geometry.coordinates' : {$near : [2.320483, 48.876236], $maxDistance : 1/111.12}}).count()
Faire une liste par ordre décroissant de type de show diffusé en fonction du nombre 
$ db.zoneDeTournage.aggregate([{$group : { _id : "$fields.type_de_tournage", total : { $sum : 1 } }} ,{ $sort : { total : 1 } }, { $out : "new"}]);
$ db.new.find({},{ total : 1});

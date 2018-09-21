# Delegate & events

## Delegate
le delegate permet de pointer vers un code, on peut alors pointer vers du code que l'on a pas codé (que les autres dev feront) les lambda sont des delegate. 

En comparaison avec des Classes, le delegate serait une "classe" dont l'instance est une méthode.

## Events
Le framework .NET gère les += -= pour les abonnements des evenements.
```` C#
Button b; 
//abonnement
b += () => {
  //code
  }; 

//pour la def de b.Click on aura quelque part dans la classe boutton :
event ______ Click;
````

## Exemple
Exemple présenté par JPP :

### Abonnement et evenements
1. Création d'un win Form
1. Creation d'un bouton "Appeler tout le monde"
1. Créer 3 boutons :
    * Abonner Pierre
    * Abonner Paul
    * Abonner Jacques
1. Créer des events pour les boutons (en utilisant le double click dans le form)
1. Création d'une classe conférence
    * Créer une méthode ``AppelerToutLeMonde``
    * Créer un event (utilisation d'un delegate) ``public event Action Abonnement;``
1. Créer une instance de ``Conference`` dans la classe ``Form``
1. Dans la méthode du click "Appeler tout le monde", appeler la méthode ``AppelerToutLeMonde();`` de la classe ``Conference`` : ``conf.AppelerToutLeMonde();``
1. Dans les méthodes des clicks d'abonnement (Pierre, Paul, Jacques...), créer les abonnements. Il existe 3 manières :
    * ````C#
            //Methode de base (delegate)
            conf.Abonnement += Conf_AbonnementDePierre; 
            //Creer une methode Conf_AbonnementDePierre() juste apres la methode du click
            private void Conf_AbonnementDePierre()
            {
               MessageBox.Show("Pierre");
            }
      ````
    * ````C#
            //Expression Lambda
            conf.Abonnement += () => { MessageBox.Show("Paul"); };
      ````
    * ````C#
            //Delegate anonyme (peu utilise depuis les lambda)
            conf.Abonnement += delegate () { MessageBox.Show("Jacques"); };
      ````
    * _**NB** : Pour pouvoir désabonner, il faut externaliser le code (l'avoir dans une méthode à part) car le framework ne peut pas reconnaitre du code lamba/delegate anonyme comme étant identique au code que l'on cherche à désasbonner. Pour desabonner du code lamba, il faudrait l'externaliser dans une variable_
1. Dans la classe ``Conference``, implémenter la méthode ``AppelerToutLeMonde()`` :
    ````C#
    if (Abonnement != null)
    {
      Abonnement();
    }
    ````

### Desabonnement

1. Créer 3 boutons de désabonnement (Idem que pour les boutons d'abonnement)
1. Créer les méthodes d'evenement liés (en double cliquant)
1. De la même manière que pour l'abonnement, Implémenter les méthodes des clicks de manière a désabonner :
    ````C#
    //Methode generee au double clic sur le bouton de desabonnement de pierre
        private void btnDesaPierre_Click(object sender, EventArgs e)
        {
            //Methode de base (delegate)
            conf.Abonnement -= Conf_AbonnementDePierre;
        }
    ````
Pour désabonner les méthodes lamba ou anonymes, il faut référencer le code (le mettre dans une méthode ou une variable), dans l'exemple de Paul, il faudrait modifier l'abonnement et le désabonnement de la sorte :
````C#
//Abonnement
private Action aboPaulLambda = () => { MessageBox.Show("Paul"); };
//Methode generere au double click
private void btnAboPaul_Click(object sender, EventArgs e)
{
   //Expression Lambda
   conf.Abonnement += aboPaulLambda;
}

//Desabonnement 
//Methode generere au double click
private void btnDesaPaul_Click(object sender, EventArgs e)
{
  //Expression Lambda
  conf.Abonnement -= aboPaulLambda;
}
````



---

copyright:
  years: 2018
lastupdated: "2018-06-04"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Analyse de jeux de données avec des modèles générés personnalisés

Watson Studio fournit l'environnement et les outils permettant de résoudre vos problèmes métier par une analyse collaborative des données. Vous pouvez choisir les outils dont vous avez besoin pour analyser, nettoyer et organiser les données. Apprenez à verser des données de flux, ou à créer, former et déployer des modèles d'apprentissage automatique. Watson Studio s'intègre à une large gamme de services {{site.data.keyword.cloud}} et au catalogue de connaissances Watson, lequel fournit une gestion de stratégie pour le contrôle des ressources, ainsi que des catalogues d'indexation pour les localiser. Pour en savoir plus, consultez la page https://dataplatform.ibm.com/.

Watson Studio est structuré autour d'une architecture basée sur un projet, laquelle organise vos ressources pour la résolution d'un problème métier. Les ressources incluent les connexions au cloud et les magasins de données sur site, les fichiers de données, les collaborateurs et des ressources telles que des modèles. Pour en savoir plus, consultez la page https://datascience.ibm.com/docs/content/getting-started/overview-ws.html?context=analytics.

## Apprentissage automatique pour {{site.data.keyword.DSX}}
{: #dsx}

Avec {{site.data.keyword.DSX}}, il est possible d'entraîner des modèles et de les déployer, puis de consommer les résultats à l'aide d'API. Ces API peuvent ensuite être utilisées dans vos applications iOS ou Swift.

Avec l'apprentissage automatique IBM Watson, une fois votre environnement défini, vous pouvez créer des modèles, les déployer dans le cloud, puis les entraîner. Pour plus d'informations, voir [Créer, déployer et entraîner des modèles avec {{site.data.keyword.pm_full}} et {{site.data.keyword.DSX}}](https://datascience.ibm.com/docs/content/analyze-data/wml-ai.html?context=analytics).

### Tutoriels
- [Générer un modèle de régression logistique avec {{site.data.keyword.pm_short}}](https://datascience.ibm.com/docs/content/analyze-data/ml-example-log-regress.html?context=analytics)
- [Générer a modèle Naive Bayes avec {{site.data.keyword.pm_short}}](https://datascience.ibm.com/docs/content/analyze-data/ml-example-naive-bayes.html?context=analytics)

## Configuration de {{site.data.keyword.DSX}} avec iOS et Swift
{: #dsx_ios}

1. Pour simplifier l'intégration des données d'identification, vous devez ajouter l'instance {{site.data.keyword.pm_short}} à votre appli iOS ou appli de back-end. Pour faciliter l'accessibilité, vos données d'identification sont incluses dans le tableau de bord de votre projet.

![Apprentissage automatique dans votre appli](images/ios-machinelearning-app.png)

2. Télécharger le code de l'appli.
3. Initialisation
  * Pour un projet iOS, du simple ajout de la ressource {{site.data.keyword.pm_short}} à votre projet iOS, les données d'identification sont instantanément injectées dans votre appli.
    Pour accéder aux données d'identification de votre application, copiez et collez le fragment de code suivant. Veillez également à ajouter le noeud final d'évaluation à votre appli, lequel se trouve sous l'onglet `Implémentation` de votre déploiement de modèle.

    ```Swift
    // The url to your model's scoring endpoint
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"

    // Your credentials
    var machineLearningUsername: String!
    var machineLearningPassword: String!

    // Machine Learning initialization
    if let contents = Bundle.main.path(forResource:"BMSCredentials", ofType: "plist"),
       let dictionary = NSDictionary(contentsOfFile: contents),
       let username = dictionary["machinelearningUsername"] as? String,
       let password = dictionary["machinelearningPassword"] as? String {

           machineLearningUsername = username
           machineLearningPassword = password

    }
    ```
    {: codeblock}

  * Pour une application côté serveur, ajoutez manuellement votre nom d'utilisateur et votre mot de passe à votre application ainsi que le noeud final d'évaluation, qui se trouve sous l'onglet `Implémentation` de votre déploiement de modèle. 

    ```Swift
    // Your Machine Learning Credentials
    let machineLearningUsername: String = "<your-ml-service-username>"
    let machineLearningPassword: String = "<your-ml-service-password>"

    // The url to your model's scoring endpoint
    let modelScoringURL: String = "<your-ml-model-scoringUrl>"
    ```
    {: codeblock}

4. Procédez à l'extraction des jetons d'accès et exécutez une analyse de prédiction sur les jeux de données de votre application avec un logiciel SDK client simple.

  ```Swift
  public class MachineLearning {

      private let username: String
      private let password: String

      public init(username: String, password: String) {
          self.username = username
          self.password = password
      }

      public static func failure(_ error: Error) {
          print("Error:", error.localizedDescription)
      }

      public func retrieveToken(failure: @escaping (Error) -> Void = MachineLearning.failure, success: @escaping (String) -> Void) {

          guard var request = createRequest(url: "https://ibm-watson-ml.mybluemix.net/v3/identity/token") else {
              failure(NSError(domain: "Could not create url", code: 1, userInfo: nil))
              return
          }

          guard let authString = (username + ":" + password).data(using: .utf8)?.base64EncodedString() else {
              failure(NSError(domain: "Could not encode credentials", code: 1, userInfo: nil))
              return
          }

          request.setValue("Basic \(authString)", forHTTPHeaderField: "Authorization")

          execute(request, failure: failure) { dict in

              guard let token = dict["token"] as? String else {
                  failure(NSError(domain: "Invalid Response", code: 1, userInfo: nil))
                  return
              }

              success(token)
          }
      }

      public func retrieveScore(_ scoringUrl: String, token: String, payload: [String: Any], failure: @escaping (Error) -> Void  = failure, success: @escaping ([String: Any]) -> Void) {

          guard let data = try? JSONSerialization.data(withJSONObject: payload, options: .prettyPrinted) else {
              failure(NSError(domain: "Could not encode data", code: 1, userInfo: nil))
              return
          }

          guard var request = createRequest(method: "POST", url: scoringUrl, body: data) else {
              failure(NSError(domain: "Could not create url", code: 1, userInfo: nil))
              return
          }

          request.setValue("Bearer " + token, forHTTPHeaderField: "Authorization")
          request.setValue("application/json", forHTTPHeaderField: "Content-Type")

          execute(request, failure: failure, success: success)
      }

      public func createRequest(method: String = "GET", url: String, body: Data? = nil) -> URLRequest? {
          guard let url = URL(string: url) else {
              return nil
          }
          var req = URLRequest(url: url)
          req.httpMethod = method
          req.httpBody = body
          return req
      }

      private func execute(_ request: URLRequest, failure: @escaping (Error) -> Void, success: @escaping ([String: Any]) -> Void) {
          URLSession.shared.dataTask(with: request) { data, _, error in

              guard let data = data, error == nil else {
                  let error = error ?? NSError(domain: "No data was returned", code: 1, userInfo: nil)
                  failure(error)
                  return
              }

              guard let body = try? JSONSerialization.jsonObject(with: data, options: .allowFragments) as? [String: Any],
                    let dict = body else {
                  failure(NSError(domain: "Could not create dictionary", code: 1, userInfo: nil))
                  return
              }

              guard let resp = response as? HTTPURLResponse , resp.statusCode == 200 else {
                 let msg = (dict["errors"] as? [[String: Any]])?.first?["message"] as? String ?? "An error occurred"
                 let code = (response as? HTTPURLResponse)?.statusCode ?? 1
                 failure(NSError(domain: msg, code: code, userInfo: nil))
                 return
             }

              success(dict)

          }.resume()
      }
  }
  ```
  {: codeblock}

### Exemple
{: #example}

**Nom du scénario :** Product line prediction

**Description du scénario :** Une entreprise qui commercialise des équipements de plein air et déploie un modèle pronostiquant l'intérêt des clients pour sa ligne de produits. Votre tâche consiste à effectuer des requêtes de score à partir du modèle déployé.

Une fois le modèle déployé, vous pouvez effectuer une analyse de prédiction à l'aide du noeud final d'évaluation.

```Swift
// The data you want to have analyzed
let examplePayload: [String: Any] = [
    "fields": ["GENDER", "AGE", "MARITAL_STATUS", "PROFESSION"],
    "values": [["M", 23, "Single", "Student"],["M", 55, "Single", "Executive"]]
]

// Your service credentials
let machineLearningUsername: String = "<your-ml-service-username>"
let machineLearningPassword: String = "<your-ml-service-password>"

// Your scoring end-point
let scoringUrl: String = "<your-model-scoring-url>"

// Define your client
let client = MachineLearning(username: username, password: password)

// Retrieve your access token and score your payload!
client.retrieveToken { token in
    client.retrieveScore(scoringUrl, token: token, payload: examplePayload) { dict in
        print(dict)
    }
}
```
{: codeblock}

## Etapes suivantes
{: #dsx_next}

Félicitations !  Vous pouvez maintenant analyser les jeux de données à l'aide de modèles d'apprentissage automatique générés personnalisés. Poursuivez sur votre lancée en découvrant les fonctionnalités offertes par {{site.data.keyword.pm_short}} à la section relative à la [sciences des données et l'apprentissage automatique](https://www.ibm.com/analytics/data-science/machine-learning).

### Liens connexes
* [{{site.data.keyword.pm_short}}](/docs/services/PredictiveModeling/index.html#using-machine-learning-with-data-science-experience)
* [{{site.data.keyword.DSX}}](https://datascience.ibm.com/)
* [Documentation {{site.data.keyword.DSX}}](https://datascience.ibm.com/docs/content/getting-started/welcome-main.html?context=analytics)
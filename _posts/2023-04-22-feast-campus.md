<!-- ---
layout: post
title:  실습 5. Feast 와 MLFlow 를 활용한 머신러닝 프로젝트 적용
date:   2023-04-22 00:00
image:  
tags:   MLFlow
---

##### MLFlow 와 Feast 를 이용한 예측 코드 생성

- MLFlow 저장된 모델로 예측하는 코드 작성 해보기
    - 저장된 모든 모델 검색 예제

    ```py
    import mlflow
    from mlflow.tracking import MlflowClient
    from pprint import pprint

    client = MlflowClient()
    for rm in client.search_registered_models():
        pprint(dict(rm), indent=4)
    ```

    ***

    - 특정 이름의 모델 검색 예제

    ```py
    import mlflow
    from mlflow.tracking import MlflowClient
    from pprint import pprint

    client = MlflowClient()
    for mv in client.search_model_versions("name='sk-learn-elasticnet-model'"):
        pprint(dict(mv), indent=4)
    ```

    ***
    - 모델 불러오기 예제

    ```py
    import mlflow
    model_name = "sk-learn-elasticnet-model"
    model_version = "1"
    m_uri = f"models:/{model_name}/{model_version}"
    model = mlflow.sklearn.load_model(m_uri)
    print(model)
    ```

    ***
    - DriverRankingPredictModel 클래스 생성

    ```py
    class DriverRankingPredictModel:
        def __init__(self, repo_path:str, m_uri:str, feature_service_name:str) -> None:
            self._model = mlflow.sklearn.load_model(m_uri)
            self._fs = feast.FeatureStore(repo_path=repo_path)
            self._fsvc = self._fs.get_feature_service(feature_service_name)

        def __call__(self, entity_df): # 호출 가능한 객체로 만들어주는 방법
                return self._predict(entity_df)

        def _predict(self, driver_ids):
            driver_features = self._fs.get_online_features(
                entity_rows=[{"driver_id": driver_id} for driver_id in driver_ids],
                features=self._fsvc
            )
            df = pd.DataFrame.from_dict(driver_features.to_dict())
            df["prediction"] = self._model.predict(df[sorted(df)])
            best_driver_id = df["driver_id"].iloc[df["prediction"].argmax()]

            return best_driver_id
    ```

    ***
    - 실행 메인코드 작성

    ```py
    if __name__ == "__main__":
        mlflow.set_tracking_uri("sqlite:///mlruns.db")
        # Change to your location
        REPO_PATH = "/home/jovyan/feature_repo"
        FEATURE_SERVICE_NAME = "driver_ranking_fv_svc"
        model_uri = "models:/sk-learn-elasticnet-model/3"

        model = DriverRankingPredictModel(REPO_PATH, model_uri, FEATURE_SERVICE_NAME)
        drivers = [1001, 1002, 1003]
        best_driver = model(drivers)
        print(f" Best predicted driver for completed trips: {best_driver}")
    ```

    ***

- 모델 Stage 변경하기

    ```py
    from mlflow.tracking import MlflowClient
    client = MlflowClient()
    client.transition_model_version_stage(
        name="sk-learn-elasticnet-model",
        version=3,
        stage="Production"
    )
    ```

    ***


 -->

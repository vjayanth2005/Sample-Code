# Sample-Code
Check out

app.controller('SearchController', ['$scope', '$http', '$routeParams', 'globalService', function ($scope, $http, $routeParams, globalService) {
        $scope.filterVisible = false;
        $scope.viewType = 'grid';
        $scope.searchTerm = $routeParams.searchTerm ? $routeParams.searchTerm : null;
        $scope.selectedFilters = {term: $scope.searchTerm};
        $scope.fetchingResults = false;
        $scope.sortBy = "Relevance";

        $scope.globalService = globalService;
        $http({
            url: "data/search/filters.json"
        }).success(function (data) {
            $scope.filters = data;
        });
        $scope.getSelectedFiltersCount = function () {
            var a = $scope.selectedFilters;
            var c = 0;
            for (i in a) {
                if (a.hasOwnProperty(i) && a[i] !== null)
                    c++;
            }
            $scope.selectedFiltersCount = c;
        };
        $scope.min = '01';
        $scope.max = '31';
        $scope.byRange = function(fieldName, minValue, maxValue){
            // if (minValue === undefined) minValue = Number.MIN_VALUE;
             //if (maxValue === undefined) maxValue = Number.MAX_VALUE;

  return function predicateFunc(item) {
    return minValue <= item[fieldName] && item[fieldName] <= maxValue;
  };
            
        };
        
        $scope.getResults = function () {
            $scope.getSelectedFiltersCount();
            $scope.fetchingResults = true;
            var p = JSON.parse(JSON.stringify($scope.selectedFilters));
            p.sortBy = $scope.sortBy;
            $http({
                url: "data/search/results.json",
                params: p
            }).success(function (data) {
                $scope.results = data;
                $scope.fetchingResults = false;
            });
        };
        $scope.getResults();
    }
]);
	
	
app.controller('ContentDetailController', ['$scope', '$http', '$routeParams', '$location', '$sce', '$timeout', 'globalService', function ($scope, $http, $routeParams, $location, $sce, $timeout, globalService) {
        $scope.contentDetail = '';
       // $scope.relatedWidth = 370;
       // $scope.ratingDetails = {};

//        $scope.addComment = function () {
//            if ($scope.selectedRating) {
//                $http({
//                    url: "data/content-detail/reviews_add.json",
//                    params: {rating: $scope.selectedRating, text: $scope.shareText}
//                }).success(function (data) {
//                });
//
//                $scope.reviews.unshift({name: "Yopesh G", img: "images/userpic-icon.png", title: 'New Comment', rating: $scope.selectedRating, comment: $scope.shareText});
//
//                $scope.shareText = '';
//                $scope.selectedRating = null;
//                jq('.star-rating-inner').trigger('set.rating', 6);
//            }
//        };

        $http({
            url: "data/search/results.json"
        }).success(function (data) {
            for (i = 0; i < data.length; i++) {
                if (data[i].id === $routeParams.id) {
                    $scope.contentDetail = data[i];
                    break;
                }
            }
            if (!$scope.contentDetail) {
                $location.path('/');
                return;
            }

            //$scope.updateFileView($scope.contentDetail.link, $scope.contentDetail.type, false);
            //$scope.ratingDetails = globalService.ratingDetails($scope.contentDetail.ratings);
        });

//        $http({
//            url: "data/content-detail/reviews.json",
//            params: {id: $routeParams.id}
//        }).success(function (data) {
//            $scope.reviews = data;
//        });

//        $http({
//            url: "data/content-detail/related.json",
//            params: {id: $routeParams.id}
//        }).success(function (data) {
//            $scope.related = data;
//        });
    }]);	
	
	
	
app.config(function ($routeProvider) {

    $routeProvider.when("/", {
        controller: "HomeController",
        templateUrl: "home.html"
    });
    
     $routeProvider.when("/search/:searchTerm?", {
        controller: "SearchController",
        templateUrl: "search-result.html"
    });
    
    $routeProvider.when("/content-detail/:id", {
        controller: "ContentDetailController",
        templateUrl: "content-detail.html"
    });


    /*$routeProvider.when("/ShowEnterpriseNetwork", {
        controller: "MainController",
        templateUrl: "ShowEnterpriseNetwork.html"
    });
    */
    
    /*
    $routeProvider.when("/logo", {
        controller: "MainController",
        templateUrl: "index.html"
         
    });
    */
    
    $routeProvider.when("/Show/:param", {
            controller: "HomeController",
            templateUrl: "home.html"
             
        });
    
    /*$routeProvider.when("/Collaboration", {
        controller: "MainController",
        templateUrl: "Collaboration.html"
    });*/
    $routeProvider.otherwise({redirectTo: "/"});	
	
});

plsrog <- function(X,class, kappa){
    
  # response variable
  Y0 <- factor(class)
  Y <- model.matrix(~ Y0 + 0)
  
  # penalized matrix
  P <- NULL
  p <- colSums(Y)
  for(i in 1:ncol(Y)){
    P <- cbind(P,Y[,i]/p[i])
  }
  P <- t(P)
  
  # differential matrix
  g <- ncol(Y)
  D <- diff(diag(1,g))
  
  X <- yarn$NIR[index,]
  
  # autoscaling
  X <- scale(X)
  Y <- scale(Y,scale=FALSE)
  
  # sample size-1
  N <- nrow(X)-1
  
  # smoothing parameter
  C <- kappa*t(Y)%*%t(P)%*%t(D)%*%D%*%P%*%Y+(1-kappa)*diag(1,g)
  
  # cholesky decomposition
  Rx <- chol(solve(C))
  Ry <- chol(C)
  
  # singular value decomposition
  USVx <- svd(Rx%*%t(Y)%*%X/N)
  USVy <- svd(t(X)%*%Y%*%solve(Ry)/N)
  
  # weght vector
  Wx <- USVx$v
  Wy <- solve(Ry)%*%USVy$v
  
  # score
  T <- X%*%Wx
  S <- Y%*%Wy
  
  all <- list(T,S,Wx,Wy)
}
